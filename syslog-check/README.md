Offline Sylog Check
=========

This Ansible playbook checks all managed hosts if syslog forwarding configured correctly.

How it Works ?
=========
Basically, what Ansible playbook does that it connects to the managed host  and checks if is there is any connection (ESTABLISHED) to syslog server on the ports listed below to IP listed below. This playbook does not check any [r]syslog.conf  as config  varies based on a GNU/Linux distrubution.

Syslog servers that is checked by ansible in the managed hosts:
==========
```bash
SYSLOG-SERVER1        10.5.100.20
SYSLOG-SERVER2        10.90.20.20
```

Syslog ports that is checked by ansible(TCP/UDP) in the managed hosts:
=========
```bash
514, 601, 35514, 9001, 9002, 9003, 9004, 9005, 9010, 9020
```

 Requirements
============
- Managed host has to has python installed. The playbook does not use raw module.
- Managed host must to have the one of the three tool **netstat || ss || lsof** . Otherwise it will fail for that managed host.
- You need to populate inventory.ini accordingly. 
- User has to be root or needs a sudo access.

Example Playbook
==============
After populate your hosts in the inventory.ini, you can run the playbook like below.

``` bash
ansible-playbook -i inventory.ini main.yaml  -u root --ask-pass 
```

If you want to limit the host or want to run on the specific hosts instead of all
``` bash
ansible-playbook -i inventory.ini main.yaml  -u root --ask-pass --limit '<hostname1> <hostname2>'
```

If you  have any ssh connection problem related to ssh host key checking, you can simply disable the ssh host key checking with environment variable. Export it before you run the playbook.

``` bash
export ANSIBLE_HOST_KEY_CHECKING=False
```


Author Information
------------------

**manintheit**

