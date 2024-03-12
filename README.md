ansible-cron
=========

This role is useful for managing crontab jobs by ansible.

Requirements
------------

crond package installed.

- **action**: *Required argument to specify the method*:
  - **add**: *Create a job.*
  - **update**: *Update an existing job.*
  - **remove**: *Remove an existing job.*

Role Variables
--------------

- cron_name: *Name for identifying the cronjob*
- cron_user: *Job owner*
- cron_job: *The command that should be executed*
- minute: *Minutes definition for the crontab*
- hour: *Hours definition for the crontab*

Example Playbook
----------------

### site.yml

```yaml
- hosts: localhost
  remote_user: root
  become: yes

  vars:
    action: "add"
    name: "test_cronjob"
    job: "echo -n 'Hello from my cron!' | tee -a /tmp/test_cronjob.log > /dev/null 2>&1"
    user: "root"
    minute: "*/2"
    hour: "*"

  roles:
    - ansible-cron
```

```yaml
ansible-playbook -i inventory.ini localhost site.yml
```

License
-------

 GPL-3.0 license

Author Information
------------------

Alex Mendes
https://www.linkedin.com/in/mendesalex/
