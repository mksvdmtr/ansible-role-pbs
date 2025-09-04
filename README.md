Proxmox-backup-server
=========

Deploys and configures Proxmox Backup Server (PBS) including repositories, packages, datastore setup, user and ACL management.
https://pbs.proxmox.com/docs/introduction.html

Requirements
------------

The role is designed to use a directory as a datastore. It is recommended to pre-configure and mount an LVM partition to any directory, the path of which should be specified in the variable `pbs_datastore_path` (default: "/pbs").

Role Variables
--------------

### Required:
```yaml
pbs_debian_version_codename: bullseye|bookworm|trixie
```
### Optional
```yaml
# if ldap realm configuring need
pbs_ldap_search_user_password: "password"
pbs_domains_cfg:
  - name: ldap
    attributes:
      comment: "LDAP (Ansible managed)"
      base-dn: "dc=domain,dc=loc"
      bind-dn: "uid=proxmox-readonly,ou=Bots,dc=domain,dc=loc"
      server1: ldap-replica.domain.loc
      user-attr: uid
      mode: ldaps
      verify: "true"
      filter: (isMemberOf=cn=Admins,ou=Groups,dc=domain,dc=loc)
      port: 1636
      sync-defaults-options: "remove-vanished=entry"
      sync-attributes: "email=mail"
      user-classes: person
      bind-password: "{{ pbs_ldap_search_user_password }}"
```
> **NOTE:** The other variables are located in defaults/main.yml, feel free to change them as needed.


Example Playbook
----------------
```yaml
- hosts: pbs
  roles:
      - { role: ansible-role-pbs }
```

