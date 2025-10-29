
* Запустить с первого раза не удалось, т.к. появлялись ошибки с `UTS`:
```shell
TASK [Create molecule instance(s)] *********************************************
changed: [localhost] => (item={'image': 'docker.io/pycontribs/centos:7', 'name': 'centos7', 'pre_build_image': True})
changed: [localhost] => (item={'image': 'docker.io/pycontribs/ubuntu:latest', 'name': 'ubuntu', 'pre_build_image': True})

TASK [Wait for instance(s) creation to complete] *******************************
failed: [localhost] (item={'started': 1, 'finished': 0, 'ansible_job_id': '68914014278.4116', 'results_file': '/root/.ansible_async/68914014278.4116', 'changed': True, 'failed': False, 'item': {'image': 'docker.io/pycontribs/centos:7', 'name': 'centos7', 'pre_build_image': True}, 'ansible_loop_var': 'item'}) => {"ansible_job_id": "68914014278.4116", "ansible_loop_var": "item", "attempts": 1, "changed": true, "cmd": ["podman", "run", "-d", "--name", "centos7", "--hostname=centos7", "docker.io/pycontribs/centos:7", "bash", "-c", "while true; do sleep 10000; done"], "delta": "0:00:00.058873", "end": "2021-12-13 19:49:45.069480", "finished": 1, "item": {"ansible_job_id": "68914014278.4116", "ansible_loop_var": "item", "changed": true, "failed": false, "finished": 0, "item": {"image": "docker.io/pycontribs/centos:7", "name": "centos7", "pre_build_image": true}, "results_file": "/root/.ansible_async/68914014278.4116", "started": 1}, "msg": "non-zero return code", "rc": 125, "start": "2021-12-13 19:49:45.010607", "stderr": "Error: invalid config provided: cannot set hostname when running in the host UTS namespace: invalid configuration", "stderr_lines": ["Error: invalid config provided: cannot set hostname when running in the host UTS namespace: invalid configuration"], "stdout": "", "stdout_lines": []}
failed: [localhost] (item={'started': 1, 'finished': 0, 'ansible_job_id': '10475541819.4138', 'results_file': '/root/.ansible_async/10475541819.4138', 'changed': True, 'failed': False, 'item': {'image': 'docker.io/pycontribs/ubuntu:latest', 'name': 'ubuntu', 'pre_build_image': True}, 'ansible_loop_var': 'item'}) => {"ansible_job_id": "10475541819.4138", "ansible_loop_var": "item", "attempts": 1, "changed": true, "cmd": ["podman", "run", "-d", "--name", "ubuntu", "--hostname=ubuntu", "docker.io/pycontribs/ubuntu:latest", "bash", "-c", "while true; do sleep 10000; done"], "delta": "0:00:00.056686", "end": "2021-12-13 19:49:45.325034", "finished": 1, "item": {"ansible_job_id": "10475541819.4138", "ansible_loop_var": "item", "changed": true, "failed": false, "finished": 0, "item": {"image": "docker.io/pycontribs/ubuntu:latest", "name": "ubuntu", "pre_build_image": true}, "results_file": "/root/.ansible_async/10475541819.4138", "started": 1}, "msg": "non-zero return code", "rc": 125, "start": "2021-12-13 19:49:45.268348", "stderr": "Error: invalid config provided: cannot set hostname when running in the host UTS namespace: invalid configuration", "stderr_lines": ["Error: invalid config provided: cannot set hostname when running in the host UTS namespace: invalid configuration"], "stdout": "", "stdout_lines": []}

PLAY RECAP *********************************************************************
localhost                  : ok=4    changed=1    unreachable=0    failed=1    skipped=3    rescued=0    ignored=0

CRITICAL Ansible return code was 2, command was: ['ansible-playbook', '--inventory', '/root/.cache/molecule/elasticsearch_role/default/inventory', '--skip-tags', 'molecule-notest,notest', '/usr/local/lib/python3.6/site-packages/molecule_podman/playbooks/create.yml']
```
* Исправила ошибку следующим образом: изменила в контейнере файл `/home/podman/.config/containers/containers.conf`, добавив значение параметра `utsns = "private"`
* Но ошибки с `ubuntu` продолжали появляться...
```shell
TASK [elasticsearch-role : Install Elasticsearch] ******************************
fatal: [ubuntu]: FAILED! => {"changed": false, "module_stderr": "sudo: unable to resolve host ubuntu\nsudo: unable to send audit message\nsudo: pam_open_session: System error\nsudo: policy plugin failed session initialization\n", "module_stdout": "", "msg": "MODULE FAILURE\nSee stdout/stderr for the exact error", "rc": 1}
```
* В результате изменила наименования хостов (на всякий) и убрала из `install_apt.yml` `become=true`
* Появилось предупреждение и ошибка конфигурации:
```shell
TASK [elasticsearch-role : Update repositories cache and install "python" package] ***
[WARNING]: Updating cache and auto-installing missing dependency: python-apt
ok: [instance-2]

TASK [elasticsearch-role : Install Elasticsearch] ******************************
changed: [instance-2]

TASK [elasticsearch-role : Update repositories] ********************************
ok: [instance-2]

TASK [elasticsearch-role : Configure Elasticsearch] ****************************
fatal: [instance-2]: FAILED! => {"msg": "Failed to get information on remote file (/etc/elasticsearch/elasticsearch.yml): sudo: unable to resolve host instance-2\nsudo: unable to send audit message\nsudo: pam_open_session: System error\nsudo: policy plugin failed session initialization\n"}
changed: [instance-1]

```
* В`install_apt.yml` и добавила `apt-update`,` apt install python-apt`:
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
  notify: restart Elasticsearch
```

* Изменила `configure.yml`:
```shell
---
- name: Configure Elasticsearch Centos
  become: true
  template:
    src: elasticsearch.yml.j2
    mode: 0644
    dest: /etc/elasticsearch/elasticsearch.yml
  notify: restart Elasticsearch
  when: ansible_facts['os_family'] != "Debian"
- name: Configure Elasticsearch Deb
  template:
    src: elasticsearch.yml.j2
    mode: 0644
    dest: /etc/elasticsearch/elasticsearch.yml
  notify: restart Elasticsearch
  when: ansible_facts['os_family'] == "Debian"
```

* Наконец, тестирование выполнилось без фатальных и критических ошибок. Привожу в отдельном [логе](01_step.md)
* Сложности возникали далее при запуске тестирования роли с kibana, разрешить на момент использования контейнера их не получалось. 
* После тестирования и использования `podman` на своей ОС решение проблем я уже знала, но возвращаться к контейнеру не стала.
