Ansible Role: PostgreSQL
=========

[![CI](https://github.com/ahu-services/ansible-role-postgresql/workflows/CI/badge.svg?event=push)](https://github.com/ahu-services/ansible-role-postgresql/actions?query=workflow%3ACI)

Installs and configures PostgtreSQL Server 15 on RockyLinux servers.

Role Variables
--------------

This role uses the original sources from postgresql.org instead of the OS source to install PostgreSQL Server version 15.
The setup is only tested on RockyLinux 8 and 9, but should also work on any other EL8/9 as well with other PostgreSQL Server version. 

Dependencies
------------

collections:
  - community.postgresql

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: database
      vars:
        postgresql_databases:
          - name: foobar # required; the rest are optional
            owner: barfoo
        postgresql_users:
          - name: barfoo # required; the rest are optional
            password: secret
            db: foobar
        postgresql_hba_entries_additional:
          - {type: host, database: foobar, user: barfoo, address: all, auth_method: "{{ postgresql_auth_method }}"}
      tasks:
        - name: Setup PostgreSQL Server
          ansible.builtin.include_role:
            name: "ahu.postgresql"

License
-------

MIT

Author Information
------------------

This role was created in 2023 by [Andreas Hubert](https://ahu.services)