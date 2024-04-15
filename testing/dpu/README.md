# OVN-Domains-Deployment


## Pre-requistes

Install the ovs ansible package
```bash
ansible-galaxy collection install openvswitch.openvswitch
```

## DPU based setup
See the included `hosts` file for a sample inventory

Run the ansible playbook as follows

```bash
ansible-playbook deploy.yaml -i hosts
```