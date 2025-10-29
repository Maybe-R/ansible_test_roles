Role Name
=========

Роль для установки kibana на хостах с ОС: Debian, Ubuntu, CentOS.

Requirements
------------

Поддерживаются только ОС семейств debian и EL.

Role Variables
--------------

| Variable name  | Default  | Description                                                       |
|----------------|----------|-------------------------------------------------------------------|
| kibana_version | "7.14.0" | Параметр, который определяет какой версии kibana будет установлен |

Dependencies
------------

| Distribution | Name host | Description                                             |
|--------------|-----------|---------------------------------------------------------|
| Centos       | el-centos | Наименование хоста для конфигурации `kibana` в `centos` |
| Ubuntu       | el-deb    | Наименование хоста для конфигурации `kibana` в `ubuntu` |          

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: all
      roles:
         - { role: lerekler-kibana-ansible }

License
-------

BSD

Author Information
------------------

An optional section for the role authors to include contact information, or a website (HTML is not allowed).
