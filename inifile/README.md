## Ansible Modify Multiple INI Sections ##

Ansible module ```ini_file``` module has limitation, when you want to modify multiple ini sections in a file.
With following Ansible playbook, the limitation has been fixed.

## Installing Collection

In order to use ```ini_file``` collection, you need to install  ```community.general``` collection.


```bash
ansible-galaxy collection install community.general
```



```yml
---
#main.yml
- hosts: localhost
  tasks:
    - name: Get Files
      shell: ls $HOME/yum.conf $HOME/yum.repos.d/*.repo
      register: files
      ignore_errors: yes

    - name: Get INI Sections on files.
      shell: egrep '^\[[a-zA-Z0-9_-]+\](\s+)?$'  "{{ item }}" | tr -d '[]'
      loop: "{{ files.stdout_lines }}"
      register: ini_sections

    - name: debug ini sections
      debug: msg="{{ ini_sections }}"

    - name: Modify ini sections
      community.general.ini_file:
        path: "{{ item.0.item }}"
        section: "{{ item.1 }}"
        option: gpgcheck
        value: 1
      with_subelements:
        - "{{ ini_sections.results }}" 
        - stdout_lines

```


## Sample ini files.

Following ini files has been provided as an example. Above Ansible playbook enable gpgchecking ```gpgcheck=1```.



```ini
#yum.conf
[main]
gpgcheck = 0
installonly_limit=3
clean_requirements_on_remove=True
best=True
skip_if_unavailable=False
```



```ini
#redhat.repo
[satellite-tools-6.9-for-rhel-8-x86_64-rpms]
gpgcheck = 1
name = Red Hat Satellite Tools 6.9 for RHEL 8 x86_64 (RPMs)
enabled = 0
gpgcheck = 0

[satellite-tools-6.9-for-rhel-8-x86_64-source-rpms]
name = Red Hat Satellite Tools 6.9 for RHEL 8 x86_64 (Source RPMs)
enabled = 0
gpgcheck = 0
enabled_metadata = 0
```
