```shell
$ molecule converge
INFO     default scenario test matrix: dependency, create, prepare, converge
INFO     Performing prerun...
INFO     Guessed /home/lerekler/Learning/gitProjects/ansible_test_roles as project root directory
WARNING  Computed fully qualified role name of kibana_role does not follow current galaxy requirements.
Please edit meta/main.yml and assure we can correctly determine full role name:

galaxy_info:
role_name: my_name  # if absent directory name hosting role is used instead
namespace: my_galaxy_namespace  # if absent, author is used instead

Namespace: https://galaxy.ansible.com/docs/contributing/namespaces.html#galaxy-namespace-limitations
Role: https://galaxy.ansible.com/docs/contributing/creating_role.html#role-names

As an alternative, you can add 'role-name' to either skip_list or warn_list.

INFO     Using /home/lerekler/.cache/ansible-lint/e43e30/roles/kibana_role symlink to current repository in order to enable Ansible to find the role using its expected full name.
INFO     Added ANSIBLE_ROLES_PATH=~/.ansible/roles:/usr/share/ansible/roles:/etc/ansible/roles:/home/lerekler/.cache/ansible-lint/e43e30/roles
INFO     Running default > dependency
WARNING  Skipping, missing the requirements file.
WARNING  Skipping, missing the requirements file.
INFO     Running default > create
INFO     Sanity checks: 'podman'

PLAY [Create] ******************************************************************

TASK [Log into a container registry] *******************************************
skipping: [localhost] => (item={'command': '/sbin/init && sleep 5000000', 'image': 'docker.io/pycontribs/centos:7', 'name': 'instance-3', 'pre_build_image': True, 'privileged': True, 'published_ports': ['0.0.0.0:5005:5005/udp', '0.0.0.0:5005:5005/tcp']})
skipping: [localhost] => (item={'command': 'sleep 5000000', 'image': 'docker.io/pycontribs/ubuntu:latest', 'name': 'instance-4', 'pre_build_image': True, 'provisioner': None, 'published_ports': ['0.0.0.0:5004:5004/udp', '0.0.0.0:5004:5004/tcp']})

TASK [Check presence of custom Dockerfiles] ************************************
ok: [localhost] => (item={'command': '/sbin/init && sleep 5000000', 'image': 'docker.io/pycontribs/centos:7', 'name': 'instance-3', 'pre_build_image': True, 'privileged': True, 'published_ports': ['0.0.0.0:5005:5005/udp', '0.0.0.0:5005:5005/tcp']})
ok: [localhost] => (item={'command': 'sleep 5000000', 'image': 'docker.io/pycontribs/ubuntu:latest', 'name': 'instance-4', 'pre_build_image': True, 'provisioner': None, 'published_ports': ['0.0.0.0:5004:5004/udp', '0.0.0.0:5004:5004/tcp']})

TASK [Create Dockerfiles from image names] *************************************
skipping: [localhost] => (item={'command': '/sbin/init && sleep 5000000', 'image': 'docker.io/pycontribs/centos:7', 'name': 'instance-3', 'pre_build_image': True, 'privileged': True, 'published_ports': ['0.0.0.0:5005:5005/udp', '0.0.0.0:5005:5005/tcp']})
skipping: [localhost] => (item={'command': 'sleep 5000000', 'image': 'docker.io/pycontribs/ubuntu:latest', 'name': 'instance-4', 'pre_build_image': True, 'provisioner': None, 'published_ports': ['0.0.0.0:5004:5004/udp', '0.0.0.0:5004:5004/tcp']})

TASK [Discover local Podman images] ********************************************
ok: [localhost] => (item={'changed': False, 'skipped': True, 'skip_reason': 'Conditional result was False', 'item': {'command': '/sbin/init && sleep 5000000', 'image': 'docker.io/pycontribs/centos:7', 'name': 'instance-3', 'pre_build_image': True, 'privileged': True, 'published_ports': ['0.0.0.0:5005:5005/udp', '0.0.0.0:5005:5005/tcp']}, 'ansible_loop_var': 'item', 'i': 0, 'ansible_index_var': 'i'})
ok: [localhost] => (item={'changed': False, 'skipped': True, 'skip_reason': 'Conditional result was False', 'item': {'command': 'sleep 5000000', 'image': 'docker.io/pycontribs/ubuntu:latest', 'name': 'instance-4', 'pre_build_image': True, 'provisioner': None, 'published_ports': ['0.0.0.0:5004:5004/udp', '0.0.0.0:5004:5004/tcp']}, 'ansible_loop_var': 'item', 'i': 1, 'ansible_index_var': 'i'})

TASK [Build an Ansible compatible image] ***************************************
skipping: [localhost] => (item={'changed': False, 'skipped': True, 'skip_reason': 'Conditional result was False', 'item': {'command': '/sbin/init && sleep 5000000', 'image': 'docker.io/pycontribs/centos:7', 'name': 'instance-3', 'pre_build_image': True, 'privileged': True, 'published_ports': ['0.0.0.0:5005:5005/udp', '0.0.0.0:5005:5005/tcp']}, 'ansible_loop_var': 'item', 'i': 0, 'ansible_index_var': 'i'})
skipping: [localhost] => (item={'changed': False, 'skipped': True, 'skip_reason': 'Conditional result was False', 'item': {'command': 'sleep 5000000', 'image': 'docker.io/pycontribs/ubuntu:latest', 'name': 'instance-4', 'pre_build_image': True, 'provisioner': None, 'published_ports': ['0.0.0.0:5004:5004/udp', '0.0.0.0:5004:5004/tcp']}, 'ansible_loop_var': 'item', 'i': 1, 'ansible_index_var': 'i'})

TASK [Determine the CMD directives] ********************************************
ok: [localhost] => (item={'command': '/sbin/init && sleep 5000000', 'image': 'docker.io/pycontribs/centos:7', 'name': 'instance-3', 'pre_build_image': True, 'privileged': True, 'published_ports': ['0.0.0.0:5005:5005/udp', '0.0.0.0:5005:5005/tcp']})
ok: [localhost] => (item={'command': 'sleep 5000000', 'image': 'docker.io/pycontribs/ubuntu:latest', 'name': 'instance-4', 'pre_build_image': True, 'provisioner': None, 'published_ports': ['0.0.0.0:5004:5004/udp', '0.0.0.0:5004:5004/tcp']})

TASK [Create molecule instance(s)] *********************************************
changed: [localhost] => (item={'command': '/sbin/init && sleep 5000000', 'image': 'docker.io/pycontribs/centos:7', 'name': 'instance-3', 'pre_build_image': True, 'privileged': True, 'published_ports': ['0.0.0.0:5005:5005/udp', '0.0.0.0:5005:5005/tcp']})
changed: [localhost] => (item={'command': 'sleep 5000000', 'image': 'docker.io/pycontribs/ubuntu:latest', 'name': 'instance-4', 'pre_build_image': True, 'provisioner': None, 'published_ports': ['0.0.0.0:5004:5004/udp', '0.0.0.0:5004:5004/tcp']})

TASK [Wait for instance(s) creation to complete] *******************************
changed: [localhost] => (item={'started': 1, 'finished': 0, 'ansible_job_id': '358345962004.167260', 'results_file': '/home/lerekler/.ansible_async/358345962004.167260', 'changed': True, 'failed': False, 'item': {'command': '/sbin/init && sleep 5000000', 'image': 'docker.io/pycontribs/centos:7', 'name': 'instance-3', 'pre_build_image': True, 'privileged': True, 'published_ports': ['0.0.0.0:5005:5005/udp', '0.0.0.0:5005:5005/tcp']}, 'ansible_loop_var': 'item'})
changed: [localhost] => (item={'started': 1, 'finished': 0, 'ansible_job_id': '718851542570.167288', 'results_file': '/home/lerekler/.ansible_async/718851542570.167288', 'changed': True, 'failed': False, 'item': {'command': 'sleep 5000000', 'image': 'docker.io/pycontribs/ubuntu:latest', 'name': 'instance-4', 'pre_build_image': True, 'provisioner': None, 'published_ports': ['0.0.0.0:5004:5004/udp', '0.0.0.0:5004:5004/tcp']}, 'ansible_loop_var': 'item'})

PLAY RECAP *********************************************************************
localhost                  : ok=5    changed=2    unreachable=0    failed=0    skipped=3    rescued=0    ignored=0

INFO     Running default > prepare
WARNING  Skipping, prepare playbook not configured.
INFO     Running default > converge

PLAY [Converge] ****************************************************************

TASK [Gathering Facts] *********************************************************
ok: [instance-4]
ok: [instance-3]

TASK [Include kibana-role] *****************************************************

TASK [kibana-role : Fail if unsupported system detected] ***********************
skipping: [instance-3]
skipping: [instance-4]

TASK [kibana-role : include_tasks] *********************************************
included: /home/lerekler/Learning/gitProjects/ansible_test_roles/playbook/roles/kibana-role/tasks/download_yum.yml for instance-3
included: /home/lerekler/Learning/gitProjects/ansible_test_roles/playbook/roles/kibana-role/tasks/download_apt.yml for instance-4

TASK [kibana-role : Download Kibana's rpm] *************************************
ok: [instance-3 -> localhost]

TASK [kibana-role : Copy Kibana to managed node] *******************************
changed: [instance-3]

TASK [kibana-role : Download Kibana's deb] *************************************
ok: [instance-4 -> localhost]

TASK [kibana-role : Copy kibana to manage host] ********************************
changed: [instance-4]

TASK [kibana-role : include_tasks] *********************************************
included: /home/lerekler/Learning/gitProjects/ansible_test_roles/playbook/roles/kibana-role/tasks/install_yum.yml for instance-3
included: /home/lerekler/Learning/gitProjects/ansible_test_roles/playbook/roles/kibana-role/tasks/install_apt.yml for instance-4

TASK [kibana-role : Update cache] **********************************************
ok: [instance-3]

TASK [kibana-role : Install Kibana] ********************************************
changed: [instance-3]

TASK [kibana-role : Update repositories cache] *********************************
changed: [instance-4]

TASK [kibana-role : Install Kibana] ********************************************
changed: [instance-4]

TASK [kibana-role : Configure Kibana] ******************************************
skipping: [instance-4]
changed: [instance-3]

TASK [kibana-role : Configure Kibana] ******************************************
skipping: [instance-3]
changed: [instance-4]

RUNNING HANDLER [kibana-role : restart Kibana Centos] **************************
changed: [instance-3]

RUNNING HANDLER [kibana-role : restart Kibana Deb] *****************************
changed: [instance-4]

PLAY RECAP *********************************************************************
instance-3                 : ok=9    changed=4    unreachable=0    failed=0    skipped=2    rescued=0    ignored=0
instance-4                 : ok=9    changed=5    unreachable=0    failed=0    skipped=2    rescued=0    ignored=0

```