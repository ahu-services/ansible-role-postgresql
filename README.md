Ansible Role: PostgreSQL
=========

[![CI](https://github.com/ahu-services/ansible-role-postgresql/workflows/CI/badge.svg?event=push)](https://github.com/ahu-services/ansible-role-postgresql/actions?query=workflow%3ACI)

Installs and configures PostgtreSQL Server 14-17 on RockyLinux servers.

Role Variables
--------------

This role uses the original sources from postgresql.org instead of the OS source to install PostgreSQL Server versions.
The setup is only tested on RockyLinux 8 and 9, but should also work on any other EL8/9 as well with other PostgreSQL Server version. 
Following OS and PostgreSQL versions have been verified:

|OS|OS Version|PostgreSQL Version|Status|
|--|---------:|-----------------:|:------:|
|Rocky Linux|8|14.19|:white_check_mark:|
|Rocky Linux|9|14.19|:white_check_mark:|
|Rocky Linux|8|15.14|:white_check_mark:|
|Rocky Linux|9|15.14|:white_check_mark:|
|Rocky Linux|8|16.10|:white_check_mark:|
|Rocky Linux|9|16.10|:white_check_mark:|
|Rocky Linux|8|17.6|:white_check_mark:|
|Rocky Linux|9|17.6|:white_check_mark:|

By default the role preloads the `pg_stat_statements` extension and installs it in the `postgres` database. Adjust or extend this behavior with `postgresql_required_shared_preload_libraries` and `postgresql_extensions`:

```
postgresql_required_shared_preload_libraries:
  - pg_stat_statements

postgresql_extensions:
  - name: pg_stat_statements
    db: postgres
    state: present
```

By default the role pins packages to the `postgresql_version` you set (e.g., `17.6`). To float to the latest patch for that major, set `postgresql_package_version` to an empty string.
To pin to a specific release as well, adjust `postgresql_package_release` (defaults to `1PGDG.rhel{{ postgresql_distribution_major_version }}`), which builds a suffix like `-17.6-1PGDG.rhel9` for the RPM names.


Dependencies
------------

collections:
  - community.postgresql

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: database
      vars:
        postgresql_version: 17.6
        postgresql_databases:
          - name: foobar # required; the rest are optional
            owner: barfoo
        postgresql_users:
          - name: barfoo # required; the rest are optional
            password: secret
            db: foobar
        postgresql_hba_entries:
          - {type: local, database: all, user: postgres, auth_method: trust}
          - {type: local, database: all, user: all, auth_method: "{{ postgresql_auth_method }}"}
          - {type: host, database: all, user: all, address: '127.0.0.1/32', auth_method: "{{ postgresql_auth_method }}"}
          - {type: host, database: all, user: all, address: '::1/128', auth_method: "{{ postgresql_auth_method }}"}        
          - {type: host, database: foobar, user: barfoo, address: all, auth_method: "{{ postgresql_auth_method }}"}
      tasks:
        - name: Setup PostgreSQL Server
          ansible.builtin.include_role:
            name: "ahu_services.postgresql"

License
-------

MIT

Author Information
------------------

This role was created in 2023 by [Andreas Hubert](https://ahu.services)
