# AnsibleDB role
This role will install all the required packages and setup all the configuration for ansibleDB.

## Quick Start

````
git clone https://github.com/dmccuk/setup_ansibledb.git
cd setup_ansibledb
````

  * Create your inventory

#### Example Inventory
````
[ansibledb]
18.168.203.188

[ansibledb:vars]
ansible_connection=ssh
ansible_user=ec2-user
ansible_ssh_private_key_file=~/.ssh/working.pem
````

  * create your deploy.yml

#### example deploy.yml
````
---
- name: run the ansibleDB role
  hosts: ansibledb
  become: true
  roles:
    - setup_ansibledb
````

## Run the role

````
$ ansible-playbook -i inventory/hosts.ini deploy.yml

PLAY [run the ansibleDB role] ********************************************************************************

TASK [Gathering Facts] ***************************************************************************************
ok: [3.8.201.79]

TASK [setup_ansibledb : loop of tasks to complete] ***********************************************************
included: /home/ec2-user/how2/setup_ansibledb/setup_ansibledb/tasks/download.yml for 3.8.201.79 => (item=download.yml)
included: /home/ec2-user/how2/setup_ansibledb/setup_ansibledb/tasks/install.yml for 3.8.201.79 => (item=install.yml)
included: /home/ec2-user/how2/setup_ansibledb/setup_ansibledb/tasks/configure.yml for 3.8.201.79 => (item=configure.yml)

TASK [setup_ansibledb : install yum-utils] *******************************************************************
ok: [3.8.201.79]

TASK [setup_ansibledb : Add Docker repository] ***************************************************************
ok: [3.8.201.79]

TASK [setup_ansibledb : Download docker-compose] *************************************************************
ok: [3.8.201.79]

TASK [setup_ansibledb : Install a list of packages with a list variable] *************************************
ok: [3.8.201.79]

TASK [setup_ansibledb : start and enable docker] *************************************************************
ok: [3.8.201.79]

TASK [setup_ansibledb : setup symlink] ***********************************************************************
ok: [3.8.201.79]

TASK [setup_ansibledb : add content to docker-compose file] **************************************************
ok: [3.8.201.79]

TASK [setup_ansibledb : docker-dompose up] *******************************************************************
changed: [3.8.201.79]

PLAY RECAP ***************************************************************************************************
3.8.201.79                 : ok=12   changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0

````

## Check it worked
Check the server is listening on port :443

````
$ sudo netstat -tnlp
Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      1092/sshd
tcp        0      0 0.0.0.0:443             0.0.0.0:*               LISTEN      29875/docker-proxy    <<--- :443
tcp        0      0 0.0.0.0:27017           0.0.0.0:*               LISTEN      29891/docker-proxy
tcp        0      0 0.0.0.0:80              0.0.0.0:*               LISTEN      19877/nginx: master
tcp6       0      0 :::21                   :::*                    LISTEN      18381/vsftpd
tcp6       0      0 :::22                   :::*                    LISTEN      1092/sshd
tcp6       0      0 :::443                  :::*                    LISTEN      29880/docker-proxy    <<--- :443
tcp6       0      0 :::27017                :::*                    LISTEN      29895/docker-proxy
tcp6       0      0 :::80                   :::*                    LISTEN
````
