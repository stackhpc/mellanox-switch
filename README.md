Mellanox Switch
===============

This role configures Mellanox switches running MLNX-OS using the
[expect](http://docs.ansible.com/ansible/latest/modules/expect_module.html)
Ansible module.

This role will install the python `expect` package to the system site packages
on the local machine.

Requirements
------------

The switches should be configured to allow SSH access.

Role Variables
--------------

`mellanox_switch_provider` is authentication provider information,
similar to the `provider` argument to the `dellos` modules. It should be a dict
containing the following fields:

- `host`: the host or IP address of the switch.
- `username`: the username with which to access the switch via SSH.
- `auth_pass`: the password with which to authenticate.

`mellanox_switch_config` is a list of configuration lines to apply to
the switch, and defaults to an empty list.

`mellanox_switch_interface_config` contains interface configuration.
It is a dict mapping switch interface names to configuration dicts. Each dict
may contain the following items:

- `description` - a description to apply to the interface.
- `config` - a list of per-interface configuration.

Dependencies
------------

None

Example Playbook
----------------

The following playbook configures hosts in the `mellanox-switches` group.  It
assumes host variables for each switch holding the host, username and
passwords.  It applies global configuration for VLAN 42, and enables two
ethernet interfaces as access mode switchports.

    ---
    - name: Ensure Mellanox switches are configured
      hosts: mellanox-switches
      gather_facts: no
      roles:
        - role: stackhpc.mellanox-switch
          mellanox_switch_provider:
            host: "{{ switch_host }}"
            username: "{{ switch_user }}"
            auth_pass: "{{ switch_auth_pass }}"
          mellanox_switch_config:
            - "vlan 42"
            - "exit"
          mellanox_switch_interface_config:
            ethernet 1/1:
              description: server-1
              config:
                - "no shutdown"
                - "switchport mode access"
            ethernet 1/2:
              description: server-2
              config:
                - "no shutdown"
                - "switchport mode access"

Author Information
------------------

- Mark Goddard (<mark@stackhpc.com>)
