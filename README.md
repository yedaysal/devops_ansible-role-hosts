devops_ansible-role-hosts
=========

An Ansible role for managing the hosts file (`/etc/hosts`). Specifically, the responsibilities of this role are to:

- Add the default localhost entry;
- Add an entry for the host name bound to the host's default external IPv4 address (optional);
- Add entries for basic IPv6 addresses, e.g. ip6-localnet (optional);
- Add entries for Ansible managed hosts (optional);
- Add entries specified in Yaml (optional, see below);
- Add entries specified in text files (optional).

Requirements
------------

This role does not have any requirements.

Role Variables
--------------

| Variable | Default | Explanation |
| --- | --- | --- |
| `hosts_add_default_ipv4` | true | If true, an entry for the host name is added, bound to the host's default IPv4 address. |
| `hosts_add_basic_ipv6` | false | If true, basic IPv6 entries are added (e.g. localhost6, ip6-localnet, etc.) |
| `hosts_add_ansible_managed_hosts` | false | If true, an entry for hosts managed by Ansible is added. (*) |
| `hosts_add_ansible_managed_hosts_groups` | ['all'] | Control which host entries are created when using `hosts_add_ansible_managed_hosts` |
| `hosts_entries` | [] | A list of dicts with custom entries to be added to the hosts file. See below for an example. |
| `hosts_file_snippets` | [] | A list of files containing host file snippets to be added to the hosts file verbatim. |
| `hosts_ip_protocol` | ipv4 | When adding Ansible managed hosts, this specifies the IP protocol (ipv4 or ipv6) |
| `hosts_network_interface` | `{{ansible_default_ipv4.interface}}` | When adding Ansible managed hosts, this specifies the network interface for which the IP address should be added. |
| `hosts_file_backup` | no | If yes, backup of host file is created with timestamp |

(*) When setting `hosts_add_ansible_managed_hosts`, an entry for the current host will also be added. Consequently, `hosts_add_default_ipv4` doesn't need to be set.

Individual hosts file entries can be added with `hosts_entries`, a list of dicts with keys `name`, `ip` and (optional) `aliases`. Example:

```yaml
hosts_entries:
  - name: slashdot
    ip: 216.34.181.45
  - name: gns1
    ip: 8.8.8.8
    aliases:
      - googledns1
      - googlens1
  - name: gns2
    ip: 8.8.4.4
    aliases:
      - googledns2
      - googlens2
```

Dependencies
------------

This role does not have any dependencies.

Example Playbook
----------------

This role is included in `devops_main/ansible/plays/base.yml`:

```yaml
- name: Ensure base config for all nodes
  hosts: all
  become: true
  roles:
    - base
    - aa_filebeat
    - aa_metricbeat
    - aa_system-configuration
    - aa_hosts
```

License
-------

There is no license for this role.

Author Information
------------------

akakce-devops (devops@akakce.com)
