# Работа с `logstash-role`

## 1. Создать новый каталог с ролью при помощи `ansible-galaxy role init logstash-role`.

```shell
$ ansible-galaxy role init logstash-role
- Role logstash-role was created successfully
```

## 2. На основе tasks из старого playbook заполните новую role. Разнесите переменные между `vars` и `default`.

* В `vars` остановимся на поддерживаемых системах:
```shell
 $ cat vars/main.yml 
---
supported_systems: ['CentOS', 'Ubuntu', 'Debian']
```
* Версия `logstash` в `defaults`:
```shell
$ cat defaults/main.yml 
---
logstash_version: "7.15.2"
logstash_install_type: remote
```
##### Распределим задачи:
* `handlers/main.yml`:
```yaml
---
- name: restart logstash
  become: true
  service:
    name: logstash
    state: restarted
```
* `configure.yml`:
```yaml
---
- name: Configure Logstash
  become: true
  template:
    src: logstash-simple.conf.j2
    mode: 0644
    dest: /etc/logstash/logstash-simple.conf
  notify: restart Logstash
- name: Run logstash
  become: true
  command:
    cmd: bin/logstash -f /etc/logstash/logstash-simple.conf
    chdir: /usr/share/logstash/bin
  register: logstash_modules
```
* `download_apt`:
```yaml
---
- name: "Download Logstash's deb"
  get_url:
    url: "https://artifacts.elastic.co/downloads/logstash/logstash-{{ logstash_version }}-amd64.deb"
    dest: "files/logstash-{{ logstash_version }}-amd64.deb"
  delegate_to: localhost
  register: download_logstash
  until: download_logstash is succeeded
  when: logstash_install_type == 'remote'
- name: Copy Logstash to manage host
  copy:
    src: "files/logstash-{{ logstash_version }}-amd64.deb"
    mode: 0755
    dest: "/tmp/logstash-{{ logstash_version }}-amd64.deb"
```

* `download_yum`:
```yaml
---
- name: "Download Logstash's rpm"
  get_url:
    url: "https://artifacts.elastic.co/downloads/logstash/logstash-{{ logstash_version }}-x86_64.rpm"
    dest: "files/logstash-{{ logstash_version }}-x86_64.rpm"
  register: download_logstash
  delegate_to: localhost
  until: download_logstash is succeeded
  when: logstash_install_type == 'remote'
- name: Copy Logstash to managed node
  copy:
    src: "files/logstash-{{ logstash_version }}-x86_64.rpm"
    mode: 0755
    dest: "/tmp/logstash-{{ logstash_version }}-x86_64.rpm"
```

* `install_apt.yml`:
```yaml
---
- name: Install Logstash
  become: true
  apt:
    deb: "/tmp/logstash-{{ logstash_version }}-amd64.deb"
    state: present
  notify: restart Logstash
```

* `install_yum`:
```yaml
---
- name: Install Logstash
  become: true
  yum:
    name: "/tmp/logstash-{{ logstash_version }}-x86_64.rpm"
    state: present
  notify: restart Logstash
```

* `precheck.yml` оставим тот же, что у `elastic`:
```yaml
---
- name: Fail if unsupported system detected
  fail:
    msg: "System {{ ansible_distribution }} is not support by this role"
  when: ansible_distribution not in supported_systems
```

* `main.yml` в `tasks`:
```yaml
---
- import_tasks: precheck.yml
- include_tasks: "download_{{ ansible_facts.pkg_mgr }}.yml"
- include_tasks: "install_{{ ansible_facts.pkg_mgr }}.yml"
- import_tasks: configure.yml
```

## 3. Перенести нужные шаблоны конфигов в `templates`.
* Пока сделала условное выражение для `centos` и `ubuntu/debian` в таком формате:
```shell
$ cat logstash-simple.conf.j2

{% if ansible_distribution == "CentOS" %}
output {
  centos {
    hosts => ["http://{{ hostvars['el-centos']['ansible_facts']['default_ipv4']['address'] }}:9200"]
  }
  stdout { codec => rubydebug }
}
{% else %}
output {
  elasticsearch {
    hosts => ["http://{{ hostvars['el-ubuntu']['ansible_facts']['default_ipv4']['address'] }}:9200"]
  }
  stdout { codec => rubydebug }
}
{% endif %}
```
## 4. Описать в `README.md` роль и её параметры. Выложите в репозиторий. Проставьте тэги, используя семантическую нумерацию.

* Ссылка на [README.md](https://github.com/lereklerik/logstash-role#role-name)

 ![tag](img.png)
