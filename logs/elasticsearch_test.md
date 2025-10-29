```shell
$ molecule test
INFO     default scenario test matrix: dependency, lint, cleanup, destroy, syntax, create, prepare, converge, idempotence, side_effect, verify, cleanup, destroy
INFO     Performing prerun...
INFO     Guessed /home/lerekler/Learning/gitProjects/ansible_test_roles/playbook/roles/elasticsearch-role as project root directory
WARNING  Computed fully qualified role name of elasticsearch_role does not follow current galaxy requirements.
Please edit meta/main.yml and assure we can correctly determine full role name:

galaxy_info:
role_name: my_name  # if absent directory name hosting role is used instead
namespace: my_galaxy_namespace  # if absent, author is used instead

Namespace: https://galaxy.ansible.com/docs/contributing/namespaces.html#galaxy-namespace-limitations
Role: https://galaxy.ansible.com/docs/contributing/creating_role.html#role-names

As an alternative, you can add 'role-name' to either skip_list or warn_list.

INFO     Using /home/lerekler/.cache/ansible-lint/2543fe/roles/elasticsearch_role symlink to current repository in order to enable Ansible to find the role using its expected full name.
INFO     Added ANSIBLE_ROLES_PATH=~/.ansible/roles:/usr/share/ansible/roles:/etc/ansible/roles:/home/lerekler/.cache/ansible-lint/2543fe/roles
INFO     Running default > dependency
WARNING  Skipping, missing the requirements file.
WARNING  Skipping, missing the requirements file.
INFO     Running default > lint
COMMAND: ansible-lint .
yamllint .

WARNING  Computed fully qualified role name of elasticsearch_role does not follow current galaxy requirements.
Please edit meta/main.yml and assure we can correctly determine full role name:

galaxy_info:
role_name: my_name  # if absent directory name hosting role is used instead
namespace: my_galaxy_namespace  # if absent, author is used instead

Namespace: https://galaxy.ansible.com/docs/contributing/namespaces.html#galaxy-namespace-limitations
Role: https://galaxy.ansible.com/docs/contributing/creating_role.html#role-names

As an alternative, you can add 'role-name' to either skip_list or warn_list.

WARNING  Computed fully qualified role name of elasticsearch_role does not follow current galaxy requirements.
Please edit meta/main.yml and assure we can correctly determine full role name:

galaxy_info:
role_name: my_name  # if absent directory name hosting role is used instead
namespace: my_galaxy_namespace  # if absent, author is used instead

Namespace: https://galaxy.ansible.com/docs/contributing/namespaces.html#galaxy-namespace-limitations
Role: https://galaxy.ansible.com/docs/contributing/creating_role.html#role-names

As an alternative, you can add 'role-name' to either skip_list or warn_list.

Loading custom .yamllint config file, this extends our internal yamllint config.
INFO     Running default > cleanup
WARNING  Skipping, cleanup playbook not configured.
INFO     Running default > destroy
INFO     Sanity checks: 'podman'

PLAY [Destroy] *****************************************************************

TASK [Destroy molecule instance(s)] ********************************************
changed: [localhost] => (item={'command': '/sbin/init && sleep 5000000', 'image': 'docker.io/pycontribs/centos:7', 'name': 'instance-1', 'pre_build_image': True, 'privileged': True, 'published_ports': ['0.0.0.0:5003:5003/udp', '0.0.0.0:5003:5003/tcp']})
changed: [localhost] => (item={'command': 'sleep 5000000', 'image': 'docker.io/pycontribs/ubuntu:latest', 'name': 'instance-2', 'pre_build_image': True, 'published_ports': ['0.0.0.0:5002:5002/udp', '0.0.0.0:5002:5002/tcp']})

TASK [Wait for instance(s) deletion to complete] *******************************
changed: [localhost] => (item={'started': 1, 'finished': 0, 'ansible_job_id': '188685769294.95181', 'results_file': '/home/lerekler/.ansible_async/188685769294.95181', 'changed': True, 'failed': False, 'item': {'command': '/sbin/init && sleep 5000000', 'image': 'docker.io/pycontribs/centos:7', 'name': 'instance-1', 'pre_build_image': True, 'privileged': True, 'published_ports': ['0.0.0.0:5003:5003/udp', '0.0.0.0:5003:5003/tcp']}, 'ansible_loop_var': 'item'})
changed: [localhost] => (item={'started': 1, 'finished': 0, 'ansible_job_id': '331879878533.95209', 'results_file': '/home/lerekler/.ansible_async/331879878533.95209', 'changed': True, 'failed': False, 'item': {'command': 'sleep 5000000', 'image': 'docker.io/pycontribs/ubuntu:latest', 'name': 'instance-2', 'pre_build_image': True, 'published_ports': ['0.0.0.0:5002:5002/udp', '0.0.0.0:5002:5002/tcp']}, 'ansible_loop_var': 'item'})

PLAY RECAP *********************************************************************
localhost                  : ok=2    changed=2    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0

INFO     Running default > syntax

playbook: /home/lerekler/Learning/gitProjects/ansible_test_roles/playbook/roles/elasticsearch-role/molecule/default/converge.yml
INFO     Running default > create

PLAY [Create] ******************************************************************

TASK [Log into a container registry] *******************************************
skipping: [localhost] => (item={'command': '/sbin/init && sleep 5000000', 'image': 'docker.io/pycontribs/centos:7', 'name': 'instance-1', 'pre_build_image': True, 'privileged': True, 'published_ports': ['0.0.0.0:5003:5003/udp', '0.0.0.0:5003:5003/tcp']})
skipping: [localhost] => (item={'command': 'sleep 5000000', 'image': 'docker.io/pycontribs/ubuntu:latest', 'name': 'instance-2', 'pre_build_image': True, 'published_ports': ['0.0.0.0:5002:5002/udp', '0.0.0.0:5002:5002/tcp']}) 

TASK [Check presence of custom Dockerfiles] ************************************
ok: [localhost] => (item={'command': '/sbin/init && sleep 5000000', 'image': 'docker.io/pycontribs/centos:7', 'name': 'instance-1', 'pre_build_image': True, 'privileged': True, 'published_ports': ['0.0.0.0:5003:5003/udp', '0.0.0.0:5003:5003/tcp']})
ok: [localhost] => (item={'command': 'sleep 5000000', 'image': 'docker.io/pycontribs/ubuntu:latest', 'name': 'instance-2', 'pre_build_image': True, 'published_ports': ['0.0.0.0:5002:5002/udp', '0.0.0.0:5002:5002/tcp']})

TASK [Create Dockerfiles from image names] *************************************
skipping: [localhost] => (item={'command': '/sbin/init && sleep 5000000', 'image': 'docker.io/pycontribs/centos:7', 'name': 'instance-1', 'pre_build_image': True, 'privileged': True, 'published_ports': ['0.0.0.0:5003:5003/udp', '0.0.0.0:5003:5003/tcp']})
skipping: [localhost] => (item={'command': 'sleep 5000000', 'image': 'docker.io/pycontribs/ubuntu:latest', 'name': 'instance-2', 'pre_build_image': True, 'published_ports': ['0.0.0.0:5002:5002/udp', '0.0.0.0:5002:5002/tcp']}) 

TASK [Discover local Podman images] ********************************************
ok: [localhost] => (item={'changed': False, 'skipped': True, 'skip_reason': 'Conditional result was False', 'item': {'command': '/sbin/init && sleep 5000000', 'image': 'docker.io/pycontribs/centos:7', 'name': 'instance-1', 'pre_build_image': True, 'privileged': True, 'published_ports': ['0.0.0.0:5003:5003/udp', '0.0.0.0:5003:5003/tcp']}, 'ansible_loop_var': 'item', 'i': 0, 'ansible_index_var': 'i'})
ok: [localhost] => (item={'changed': False, 'skipped': True, 'skip_reason': 'Conditional result was False', 'item': {'command': 'sleep 5000000', 'image': 'docker.io/pycontribs/ubuntu:latest', 'name': 'instance-2', 'pre_build_image': True, 'published_ports': ['0.0.0.0:5002:5002/udp', '0.0.0.0:5002:5002/tcp']}, 'ansible_loop_var': 'item', 'i': 1, 'ansible_index_var': 'i'})

TASK [Build an Ansible compatible image] ***************************************
skipping: [localhost] => (item={'changed': False, 'skipped': True, 'skip_reason': 'Conditional result was False', 'item': {'command': '/sbin/init && sleep 5000000', 'image': 'docker.io/pycontribs/centos:7', 'name': 'instance-1', 'pre_build_image': True, 'privileged': True, 'published_ports': ['0.0.0.0:5003:5003/udp', '0.0.0.0:5003:5003/tcp']}, 'ansible_loop_var': 'item', 'i': 0, 'ansible_index_var': 'i'})
skipping: [localhost] => (item={'changed': False, 'skipped': True, 'skip_reason': 'Conditional result was False', 'item': {'command': 'sleep 5000000', 'image': 'docker.io/pycontribs/ubuntu:latest', 'name': 'instance-2', 'pre_build_image': True, 'published_ports': ['0.0.0.0:5002:5002/udp', '0.0.0.0:5002:5002/tcp']}, 'ansible_loop_var': 'item', 'i': 1, 'ansible_index_var': 'i'})

TASK [Determine the CMD directives] ********************************************
ok: [localhost] => (item={'command': '/sbin/init && sleep 5000000', 'image': 'docker.io/pycontribs/centos:7', 'name': 'instance-1', 'pre_build_image': True, 'privileged': True, 'published_ports': ['0.0.0.0:5003:5003/udp', '0.0.0.0:5003:5003/tcp']})
ok: [localhost] => (item={'command': 'sleep 5000000', 'image': 'docker.io/pycontribs/ubuntu:latest', 'name': 'instance-2', 'pre_build_image': True, 'published_ports': ['0.0.0.0:5002:5002/udp', '0.0.0.0:5002:5002/tcp']})

TASK [Create molecule instance(s)] *********************************************
changed: [localhost] => (item={'command': '/sbin/init && sleep 5000000', 'image': 'docker.io/pycontribs/centos:7', 'name': 'instance-1', 'pre_build_image': True, 'privileged': True, 'published_ports': ['0.0.0.0:5003:5003/udp', '0.0.0.0:5003:5003/tcp']})
changed: [localhost] => (item={'command': 'sleep 5000000', 'image': 'docker.io/pycontribs/ubuntu:latest', 'name': 'instance-2', 'pre_build_image': True, 'published_ports': ['0.0.0.0:5002:5002/udp', '0.0.0.0:5002:5002/tcp']})

TASK [Wait for instance(s) creation to complete] *******************************
changed: [localhost] => (item={'started': 1, 'finished': 0, 'ansible_job_id': '523164039256.95565', 'results_file': '/home/lerekler/.ansible_async/523164039256.95565', 'changed': True, 'failed': False, 'item': {'command': '/sbin/init && sleep 5000000', 'image': 'docker.io/pycontribs/centos:7', 'name': 'instance-1', 'pre_build_image': True, 'privileged': True, 'published_ports': ['0.0.0.0:5003:5003/udp', '0.0.0.0:5003:5003/tcp']}, 'ansible_loop_var': 'item'})
changed: [localhost] => (item={'started': 1, 'finished': 0, 'ansible_job_id': '841405789080.95593', 'results_file': '/home/lerekler/.ansible_async/841405789080.95593', 'changed': True, 'failed': False, 'item': {'command': 'sleep 5000000', 'image': 'docker.io/pycontribs/ubuntu:latest', 'name': 'instance-2', 'pre_build_image': True, 'published_ports': ['0.0.0.0:5002:5002/udp', '0.0.0.0:5002:5002/tcp']}, 'ansible_loop_var': 'item'})

PLAY RECAP *********************************************************************
localhost                  : ok=5    changed=2    unreachable=0    failed=0    skipped=3    rescued=0    ignored=0

INFO     Running default > prepare
WARNING  Skipping, prepare playbook not configured.
INFO     Running default > converge

PLAY [Converge] ****************************************************************

TASK [Gathering Facts] *********************************************************
ok: [instance-2]
ok: [instance-1]

TASK [Include elasticsearch-role] **********************************************

TASK [elasticsearch-role : Fail if unsupported system detected] ****************
skipping: [instance-1]
skipping: [instance-2]

TASK [elasticsearch-role : include_tasks] **************************************
included: /home/lerekler/Learning/gitProjects/ansible_test_roles/playbook/roles/elasticsearch-role/tasks/download_yum.yml for instance-1
included: /home/lerekler/Learning/gitProjects/ansible_test_roles/playbook/roles/elasticsearch-role/tasks/download_apt.yml for instance-2

TASK [elasticsearch-role : Download Elasticsearch's rpm] ***********************
ok: [instance-1 -> localhost]

TASK [elasticsearch-role : Copy Elasticsearch to managed node] *****************
changed: [instance-1]

TASK [elasticsearch-role : Download Elasticsearch's deb] ***********************
ok: [instance-2 -> localhost]

TASK [elasticsearch-role : Copy Elasticsearch to manage host] ******************
changed: [instance-2]

TASK [elasticsearch-role : include_tasks] **************************************
included: /home/lerekler/Learning/gitProjects/ansible_test_roles/playbook/roles/elasticsearch-role/tasks/install_yum.yml for instance-1
included: /home/lerekler/Learning/gitProjects/ansible_test_roles/playbook/roles/elasticsearch-role/tasks/install_apt.yml for instance-2

TASK [elasticsearch-role : Update APT package manager repositories cache] ******
ok: [instance-1]

TASK [elasticsearch-role : install EPEL repo] **********************************
ok: [instance-1]

TASK [elasticsearch-role : install python] *************************************
changed: [instance-1]

TASK [elasticsearch-role : Install Java using Ansible] *************************
changed: [instance-1]

TASK [elasticsearch-role : Install Elasticsearch] ******************************
changed: [instance-1]

TASK [elasticsearch-role : Update repositories cache and install "python" package] ***
changed: [instance-2]

TASK [elasticsearch-role : Install Elasticsearch] ******************************
changed: [instance-2]

TASK [elasticsearch-role : Configure Elasticsearch] ****************************
skipping: [instance-2]
changed: [instance-1]

TASK [elasticsearch-role : Configure Elasticsearch Deb] ************************
skipping: [instance-1]
changed: [instance-2]

RUNNING HANDLER [elasticsearch-role : restart Elasticsearch Centos] ************
changed: [instance-1]

RUNNING HANDLER [elasticsearch-role : restart Elasticsearch Deb] ***************
skipping: [instance-2]

PLAY RECAP *********************************************************************
instance-1                 : ok=12   changed=6    unreachable=0    failed=0    skipped=2    rescued=0    ignored=0
instance-2                 : ok=8    changed=4    unreachable=0    failed=0    skipped=3    rescued=0    ignored=0

INFO     Running default > idempotence

PLAY [Converge] ****************************************************************

TASK [Gathering Facts] *********************************************************
ok: [instance-2]
ok: [instance-1]

TASK [Include elasticsearch-role] **********************************************

TASK [elasticsearch-role : Fail if unsupported system detected] ****************
skipping: [instance-1]
skipping: [instance-2]

TASK [elasticsearch-role : include_tasks] **************************************
included: /home/lerekler/Learning/gitProjects/ansible_test_roles/playbook/roles/elasticsearch-role/tasks/download_yum.yml for instance-1
included: /home/lerekler/Learning/gitProjects/ansible_test_roles/playbook/roles/elasticsearch-role/tasks/download_apt.yml for instance-2

TASK [elasticsearch-role : Download Elasticsearch's rpm] ***********************
ok: [instance-1 -> localhost]

TASK [elasticsearch-role : Copy Elasticsearch to managed node] *****************
ok: [instance-1]

TASK [elasticsearch-role : Download Elasticsearch's deb] ***********************
ok: [instance-2 -> localhost]

TASK [elasticsearch-role : Copy Elasticsearch to manage host] ******************
ok: [instance-2]

TASK [elasticsearch-role : include_tasks] **************************************
included: /home/lerekler/Learning/gitProjects/ansible_test_roles/playbook/roles/elasticsearch-role/tasks/install_yum.yml for instance-1
included: /home/lerekler/Learning/gitProjects/ansible_test_roles/playbook/roles/elasticsearch-role/tasks/install_apt.yml for instance-2

TASK [elasticsearch-role : Update APT package manager repositories cache] ******
ok: [instance-1]

TASK [elasticsearch-role : install EPEL repo] **********************************
ok: [instance-1]

TASK [elasticsearch-role : install python] *************************************
ok: [instance-1]

TASK [elasticsearch-role : Install Java using Ansible] *************************
ok: [instance-1]

TASK [elasticsearch-role : Install Elasticsearch] ******************************
ok: [instance-1]

TASK [elasticsearch-role : Update repositories cache and install "python" package] ***
ok: [instance-2]

TASK [elasticsearch-role : Install Elasticsearch] ******************************
ok: [instance-2]

TASK [elasticsearch-role : Configure Elasticsearch] ****************************
skipping: [instance-2]
ok: [instance-1]

TASK [elasticsearch-role : Configure Elasticsearch Deb] ************************
skipping: [instance-1]
ok: [instance-2]

PLAY RECAP *********************************************************************
instance-1                 : ok=11   changed=0    unreachable=0    failed=0    skipped=2    rescued=0    ignored=0
instance-2                 : ok=8    changed=0    unreachable=0    failed=0    skipped=2    rescued=0    ignored=0

INFO     Idempotence completed successfully.
INFO     Running default > side_effect
WARNING  Skipping, side effect playbook not configured.
INFO     Running default > verify
INFO     Running Ansible Verifier

PLAY [Verify] ******************************************************************

TASK [Example assertion] *******************************************************
ok: [instance-1] => {
    "changed": false,
    "msg": "All assertions passed"
}
ok: [instance-2] => {
    "changed": false,
    "msg": "All assertions passed"
}

PLAY RECAP *********************************************************************
instance-1                 : ok=1    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
instance-2                 : ok=1    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0

INFO     Verifier completed successfully.
INFO     Running default > cleanup
WARNING  Skipping, cleanup playbook not configured.
INFO     Running default > destroy

PLAY [Destroy] *****************************************************************

TASK [Destroy molecule instance(s)] ********************************************
changed: [localhost] => (item={'command': '/sbin/init && sleep 5000000', 'image': 'docker.io/pycontribs/centos:7', 'name': 'instance-1', 'pre_build_image': True, 'privileged': True, 'published_ports': ['0.0.0.0:5003:5003/udp', '0.0.0.0:5003:5003/tcp']})
changed: [localhost] => (item={'command': 'sleep 5000000', 'image': 'docker.io/pycontribs/ubuntu:latest', 'name': 'instance-2', 'pre_build_image': True, 'published_ports': ['0.0.0.0:5002:5002/udp', '0.0.0.0:5002:5002/tcp']})

TASK [Wait for instance(s) deletion to complete] *******************************
FAILED - RETRYING: Wait for instance(s) deletion to complete (300 retries left).
changed: [localhost] => (item={'started': 1, 'finished': 0, 'ansible_job_id': '688913178724.115220', 'results_file': '/home/lerekler/.ansible_async/688913178724.115220', 'changed': True, 'failed': False, 'item': {'command': '/sbin/init && sleep 5000000', 'image': 'docker.io/pycontribs/centos:7', 'name': 'instance-1', 'pre_build_image': True, 'privileged': True, 'published_ports': ['0.0.0.0:5003:5003/udp', '0.0.0.0:5003:5003/tcp']}, 'ansible_loop_var': 'item'})
FAILED - RETRYING: Wait for instance(s) deletion to complete (300 retries left).
changed: [localhost] => (item={'started': 1, 'finished': 0, 'ansible_job_id': '888404274845.115248', 'results_file': '/home/lerekler/.ansible_async/888404274845.115248', 'changed': True, 'failed': False, 'item': {'command': 'sleep 5000000', 'image': 'docker.io/pycontribs/ubuntu:latest', 'name': 'instance-2', 'pre_build_image': True, 'published_ports': ['0.0.0.0:5002:5002/udp', '0.0.0.0:5002:5002/tcp']}, 'ansible_loop_var': 'item'})

PLAY RECAP *********************************************************************
localhost                  : ok=2    changed=2    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0

INFO     Pruning extra files from scenario ephemeral directory

```
