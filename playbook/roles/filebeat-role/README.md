Role Name
=========

Роль для установки filebeat на хостах с ОС: Debian, Ubuntu, CentOS.

Requirements
------------

Поддерживаются только ОС семейств debian и EL.

Role Variables
--------------

| Variable name    | Default  | Description                                                           |
|------------------|----------|-----------------------------------------------------------------------|
| filebeat_version | "7.15.2" | Параметр, который определяет какой версии `filebeat` будет установлен |

Dependencies
------------
| Distribution | Name hosts          | Description                                               |
|--------------|---------------------|-----------------------------------------------------------|
| Centos       | el-centos, k-centos | Наименование хоста для конфигурации `filebeat` в `centos` |
| Ubuntu       | el-deb, k-deb       | Наименование хоста для конфигурации `filebeat` в `ubuntu` | 

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: all
      roles:
         - { role: lerekler-filebeat-ansible }

License
-------

BSD

Author Information
------------------

An optional section for the role authors to include contact information, or a website (HTML is not allowed).
