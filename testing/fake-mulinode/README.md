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
ansible-playbook playbook.yaml -i hosts -e "@variables.yaml"
```
This playbook will expect to have some varialbes in order to run, for example setting the image of the service
it could be done by setting a variables file or specifically setting the image in the command line.
Same is done for the container registry.

The run will intall needed packages in order to run containers and docker-compose
Will build and run fake ovn multinode
Deploy the DB and the service

on success you should see all the conatiners running, example:

```sh
docker ps
CONTAINER ID   IMAGE                                                                            COMMAND                  CREATED          STATUS                    PORTS                                       NAMES
20415dda1efb   harbor.mellanox.com/cloud-orchestration-dev/mfilanov/ovn-domain-service:latest   "/ovn-domain-service"    26 minutes ago   Up 26 minutes                                                         b_ovn-domain-service_1
293dcc54ffc1   postgres                                                                         "docker-entrypoint.s…"   32 minutes ago   Up 31 minutes (healthy)                                               b_postgres_1
7a0d89f5d7a6   927a8c8d1e1d                                                                     "/usr/sbin/init"         2 days ago       Up 2 days                                                             ovn-chassis-2
f9d6c6421826   927a8c8d1e1d                                                                     "/usr/sbin/init"         2 days ago       Up 2 days                                                             ovn-chassis-1
b24245525698   927a8c8d1e1d                                                                     "/usr/sbin/init"         2 days ago       Up 2 days                                                             ovn-gw-1
c43e1f8838b8   927a8c8d1e1d                                                                     "/usr/sbin/init"         2 days ago       Up 2 days                 0.0.0.0:6641->6641/tcp, :::6641->6641/tcp   ovn-central-az1-1
```