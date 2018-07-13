Role Name
=========

A library which manages log rotate configurations.
Inspired from: https://github.com/retr0h/ansible-logrotate

Requirements
------------

Any pre-requisites that may not be covered by Ansible itself or the role should be mentioned here. For instance, if the role uses the EC2 module, it may be a good idea to mention in this section that the boto package is required.

Role Variables
--------------

A description of the settable variables for this role should go here, including any variables that are in defaults/main.yml, vars/main.yml, and any variables that can/should be set via parameters to the role. Any variables that are read from other roles and/or the global scope (ie. hostvars, group vars, etc.) should be mentioned here as well.

Dependencies
------------

A list of other roles hosted on Galaxy should go here, plus any details in regards to parameters that may need to be set for other roles, or variables that are used from other roles.

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: servers
      roles:
         - { role: username.rolename, x: 42 }

---
- hosts: all
  tasks:
    - name: Create a test logrotate config file
      logrotate: name=test
                 path=/var/log/test.log
      args:
        options:
          - daily
          - rotate 8
        scripts:
          postrotate: exec script

    - name: Create a test logrotate config file to delete
      logrotate: name=missing
                 path=/var/log/missing.log
      changed_when: False

    - name: Stat path to test logrotate conf
      stat: path=/etc/logrotate.d/test
      register: logrotate_stat
    - name: Assert that test logrotate conf exists
      assert:
        that:
          - "logrotate_stat.stat.exists == True"

    - name: Remove the test logrotate config file
      logrotate: name=missing
                 path=/var/log/missing.log
                 state=absent
      changed_when: False

    - name: Rotate list of files with default options
      logrotate: name=test_multi
      args:
        path:
          - /var/log/log1.log
          - /var/log/log2.log

License
-------

BSD

Author Information
------------------

An optional section for the role authors to include contact information, or a website (HTML is not allowed).
