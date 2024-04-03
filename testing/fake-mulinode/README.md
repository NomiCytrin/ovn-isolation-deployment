# setup

Befor starting the deployment need to make sure that host have ssh access to remote host
This part can be done by adding your ssh public key to `~/.ssh/authorized_keys`, on some system this file need to be created.

Create a hosts file, any file name will work, in my case the file is called hosts
Grouping hosts under a specific name will help to work on multiple hosts 

```
[cloud]
<host-domain-name-or-ip>

[cloud:vars]
ansible_python_interpreter=/usr/bin/python3
```

Verify connectivity with 
```sh
ansible all -m ping -i hosts
```

Valid reply will look similair to:
```sh
ansible all -m ping -i hosts
c-236-2-60-063 | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
```

# run

```sh
ansible-playbook playbook.yaml -i hosts
```

The run will intall needed packages in order to run containers and docker-compose
Will build and run fake ovn multinode
Deploy the DB of the service