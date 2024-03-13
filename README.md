ansible-cron
=========

This role is useful for managing crontab jobs by ansible.

```yaml
# For details see man 4 crontabs

# Example of job definition:
# .---------------- minute (0 - 59)
# |  .------------- hour (0 - 23)
# |  |  .---------- day of month (1 - 31)
# |  |  |  .------- month (1 - 12) OR jan,feb,mar,apr ...
# |  |  |  |  .---- day of week (0 - 6) (Sunday=0 or 7) OR sun,mon,tue,wed,thu,fri,sat
# |  |  |  |  |
# *  *  *  *  * user-name  command to be executed
```

Requirements
------------

crond package installed.

- **action**: *Required argument to specify the method*:
  - **add**: *Create a job.*
  - **addfile**: *Create a job by file (/etc/cron.d).*
  - **update**: *Update an existing job.*
  - **remove**: *Remove an existing job.*

Role Variables
--------------

- **cron_name**: *Name for identifying the cronjob*
- **cron_user**: *Job owner*
- **cron_job_cmd**: *The command that should be executed*
- **minute**: *Minutes definition for the crontab*
- **hour**: *Hours definition for the crontab*

Example Playbook
----------------

`site.yml` execution:

```yaml
ansible-playbook -i inventory.ini site.yml
```

Usage Examples
--------------

**Add job**

```yaml
- hosts: db
  remote_user: root
  become: yes
     
  vars:
    action: remove
    cron_name: "testcron"
    cron_job_cmd: "echo -n 'Hello from my cron!' | tee -a /tmp/test_cronjob.log > /dev/null 2>&1"
    cron_user: "sysadmin"
    minute: "*/1"
    hour: "*"
     
  roles:
    - ansible-cron
     
  tasks:
    - name: Print variable values
      debug:
        msg: "Value of action: {{ action }}, name: {{ cron_name }}, job: {{ cron_job_cmd }}, user: {{ cron_user }}, minute: {{ minute }}, hour: {{ hour }}"
```

**Remove job**

```yaml
- hosts: db
  remote_user: root
  become: yes

  vars:
    action: remove
    cron_name: "testcron"
    cron_user: "root"

  roles:
    - ansible-cron
```

**Remove job file (/etc/cron.d)**

```yaml
- hosts: db
  remote_user: root
  become: yes

  vars:
    action: removefile
    cron_name: "testcron"
    cron_job_file: "testcron.sh"

  roles:
    - ansible-cron
```


License
-------

 GPL-3.0 license

Author Information
------------------

Alex Mendes
https://www.linkedin.com/in/mendesalex/
