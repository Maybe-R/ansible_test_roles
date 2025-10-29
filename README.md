# Домашнее задание к занятию "08.05 Тестирование Roles"

----------------------------------------------------------
## Подготовка к выполнению

#### 1. Установите molecule: `pip3 install "molecule==3.4.0"`
```shell
$ pip3 install "molecule==3.4.0"
...
Installing collected packages: MarkupSafe, text-unidecode, Jinja2, charset-normalizer, arrow, requests, python-slugify, poyo, jinja2-time, click, binaryornot, subprocess-tee, pluggy, cookiecutter, click-help-colors, cerberus, molecule
Successfully installed Jinja2-3.0.3 MarkupSafe-2.0.1 arrow-1.2.1 binaryornot-0.4.4 cerberus-1.3.2 charset-normalizer-2.0.9 click-8.0.3 click-help-colors-0.9.1 cookiecutter-1.7.3 jinja2-time-0.2.0 molecule-3.4.0 pluggy-0.13.1 poyo-0.5.0 python-slugify-5.0.2 requests-2.26.0 subprocess-tee-0.3.5 text-unidecode-1.3
```

#### 2. Соберите локальный образ на основе [Dockerfile](Dockerfile)
```shell
$ docker build -t redhat_ansible:latest .
...
Step 14/14 : RUN rm -rf Python-*
 ---> Running in e561dcab2a1b
Removing intermediate container e561dcab2a1b
 ---> 2e0f274b5164
Successfully built 2e0f274b5164
Successfully tagged redhat_ansible:latest

$ docker images
REPOSITORY                        TAG       IMAGE ID       CREATED          SIZE
redhat_ansible                    latest    2e0f274b5164   48 seconds ago   2.48GB
```
* Однако в дальнейшем я отказалась от использования контейнера с образом RHEL, т.к. возникали постоянно [сложности](logs/00_failurewithrhel.md) с настройками. 
* Моя система - `ubuntu 20.04`, работать буду с ней и с `podman`
----------------------------------------------------------
## Основная часть

----------------------------------------------------------
### Molecule

#### 1. Запустите molecule test внутри корневой директории elasticsearch-role, посмотрите на вывод команды.

* Ошибки были, в основном, следующие:
```shell
TASK [elasticsearch-role : Configure Elasticsearch Deb] ************************
skipping: [centos7]
changed: [ubuntu]

RUNNING HANDLER [elasticsearch-role : restart Elasticsearch] *******************
fatal: [centos7]: FAILED! => {"changed": false, "msg": "Service is in unknown state", "status": {}}
changed: [ubuntu]
```
```shell
TASK [elasticsearch-role : Configure Elasticsearch Deb] ************************
skipping: [centos7]
changed: [ubuntu]

RUNNING HANDLER [elasticsearch-role : restart Elasticsearch] *******************
fatal: [centos7]: FAILED! => {"changed": false, "msg": "Unable to start service elasticsearch: Job for elasticsearch.service failed because a fatal signal was delivered to the control process. See \"systemctl status elasticsearch.service\" and \"journalctl -xe\" for details.\n"}
changed: [ubuntu]
```
* В результате, были изменены следующие файлы:
* `molecule.yml`
```yaml
---
dependency:
  name: galaxy
driver:
  name: podman
lint: |
  ansible-lint .
  yamllint .
platforms:
  - name: instance-1
    image: docker.io/pycontribs/centos:7
    command: /sbin/init && sleep 5000000
    privileged: True
    published_ports:
      - 0.0.0.0:5003:5003/udp
      - 0.0.0.0:5003:5003/tcp
    pre_build_image: true
  - name: instance-2
    image: docker.io/pycontribs/ubuntu:latest
    command: sleep 5000000
    published_ports:
      - 0.0.0.0:5002:5002/udp
      - 0.0.0.0:5002:5002/tcp
    pre_build_image: true
provisioner:
  name: ansible
verifier:
  name: ansible

```
* `tasks/install_apt.yml`:
```yaml
---
- name: Update repositories cache and install "python" package
  apt:
    name: python-apt
    update_cache: yes
- name: Install Elasticsearch
  apt:
    deb: "/tmp/elasticsearch-{{ elasticsearch_version }}-amd64.deb"
    state: present
  notify: restart Elasticsearch Deb
```
* `tasks/install_yum.yml`:
```yaml
---
- name: Update APT package manager repositories cache
  become: true
  yum:
    update_cache: yes
- name: install EPEL repo
  become: true
  yum:
    name: epel-release
    state: present
- name: install python
  become: true
  yum:
    name: python36
    state: present
- name: Install Java using Ansible
  become: yes
  yum:
    name: "{{ packages }}"
    state: present
  vars:
    packages:
      - java-11-openjdk-devel.x86_64
- name: Install Elasticsearch
  yum:
    name: "/tmp/elasticsearch-{{ elasticsearch_version }}-x86_64.rpm"
    state: present
  notify: restart Elasticsearch Centos
```
* `tasks/configure.yml`:
```yaml
---
- name: Configure Elasticsearch
  become: true
  template:
    src: elasticsearch.yml.j2
    mode: 0644
    dest: /etc/elasticsearch/elasticsearch.yml
  environment:
    - ES_JAVA_OPTS: "-Djna.tmpdir=/var/log/elasticsearch"
  notify: restart Elasticsearch Centos
  when: ansible_facts.pkg_mgr == "yum"
- name: Configure Elasticsearch Deb
  template:
    src: elasticsearch.yml.j2
    mode: 0644
    dest: /etc/elasticsearch/elasticsearch.yml
  notify: restart Elasticsearch Deb
  when: ansible_facts.pkg_mgr == "apt"
```
* `handlers/main.yml`:
```shell
---
- name: restart Elasticsearch Centos
  become: true
  service:
    name: elasticsearch
    state: restarted
  when: ansible_facts.pkg_mgr == "yum"
- name: restart Elasticsearch Deb
  service:
    name: elasticsearch
    state: restarted
  when: ansible_facts.pkg_mgr == "deb"
```
* Лог тестирования `elasticsearch-role` вынесла в отдельный [файл](logs/elasticsearch_test.md)
* Лог запуска с `molecule converge` также в отдельном [файле](logs/elasticsearch_converge.md)

```shell
$ podman ps
CONTAINER ID  IMAGE                               COMMAND               CREATED      STATUS          PORTS                                           NAMES
31955625b8bb  docker.io/pycontribs/centos:7       /sbin/init && sle...  4 hours ago  Up 4 hours ago  0.0.0.0:5003->5003/tcp, 0.0.0.0:5003->5003/udp  instance-1
719ea99b7eba  docker.io/pycontribs/ubuntu:latest  sleep 5000000         4 hours ago  Up 4 hours ago  0.0.0.0:5002->5002/tcp, 0.0.0.0:5002->5002/udp  instance-2
```
----------------------------------------------------------
#### 2. Перейдите в каталог с ролью `kibana-role` и создайте сценарий тестирования по умолчанию при помощи `molecule init scenario --driver-name docker`.

* Учитывая, что работаю с `podman`, создавать сценарий буду с `--driver-name podman`
```shell
$ molecule init scenario --driver-name podman
INFO     Initializing new scenario default...
INFO     Initialized scenario in /home/lerekler/Learning/gitProjects/ansible_test_roles/playbook/roles/kibana-role/molecule/default successfully.
```

#### 3. Добавьте несколько разных дистрибутивов (centos:8, ubuntu:latest) для инстансов и протестируйте роль, исправьте найденные ошибки, если они есть.

* `molecule.yml`:
```yaml
---
dependency:
  name: galaxy
driver:
  name: podman
  managed: false
  login_cmd_template: sleep 5000000
lint: |
  ansible-lint .
  yamllint .
platforms:
  - name: instance-3
    image: docker.io/pycontribs/centos:7
    command: /sbin/init && sleep 5000000
    privileged: True
    published_ports:
      - 0.0.0.0:5005:5005/udp
      - 0.0.0.0:5005:5005/tcp
    pre_build_image: true
  - name: instance-4
    image: docker.io/pycontribs/ubuntu:latest
    command: sleep 5000000
    published_ports:
      - 0.0.0.0:5004:5004/udp
      - 0.0.0.0:5004:5004/tcp
    pre_build_image: true
    provisioner:
provisioner:
  name: ansible
verifier:
  name: ansible
```
* `install_yum.yml`:
```yaml
---
- name: Update cache
  become: true
  yum:
    update_cache: yes
- name: Install Kibana
  become: true
  yum:
    name: "/tmp/kibana-{{ kibana_version }}-x86_64.rpm"
    state: present
  notify: restart Kibana Centos
```
* `install_apt.yml`:
```yaml
---
- name: Update repositories cache
  apt:
    update_cache: yes
- name: Install Kibana
  apt:
    deb: "/tmp/kibana-{{ kibana_version }}-amd64.deb"
    state: present
  notify: restart Kibana Deb
```
* `configure.yml`:
```yaml
---
- name: Configure Kibana
  become: true
  template:
    src: kibana.yml.j2
    dest: /etc/kibana/kibana.yml
    mode: 0644
  notify: restart Kibana Centos
  when: ansible_facts.pkg_mgr == "yum"
- name: Configure Kibana
  template:
    src: kibana.yml.j2
    dest: /etc/kibana/kibana.yml
    mode: 0644
  notify: restart Kibana Deb
  when: ansible_facts.pkg_mgr == "apt"
```
* `handlers/main.yml`:
```yaml
---
- name: restart Kibana Centos
  become: true
  service:
    name: kibana
    state: restarted
  when: ansible_facts.pkg_mgr == "yum"
- name: restart Kibana Deb
  service:
    name: kibana
    state: restarted
  when: ansible_facts.pkg_mgr == "apt"
```
* [Лог](logs/kibana_test_01.md) тестирования
* [`molecule converge`](logs/kibana_converge_01.md)


#### 4. Добавьте несколько assert'ов в verify.yml файл, для проверки работоспособности kibana-role (проверка, что web отвечает, проверка логов, etc). Запустите тестирование роли повторно и проверьте, что оно прошло успешно.