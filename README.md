# Ansible Role: zrepl

![Build Status](https://github.com/transitiv/ansible-role-zrepl/workflows/CI/badge.svg)

This role installs and configures [zrepl](https://zrepl.github.io/) on systems
running Debian/Ubuntu and CentOS/RHEL.

## Requirements

Root accesss is required to add repositories and install packages, so the role
must either be run in a playbook with global root privileges or set `become:
yes` when it is included:

```yaml
- hosts: zfs_servers
  roles:
    - role: transitiv.zrepl
      become: yes
```

## Role Variables

```yaml
zrepl_config_global:
  logging:
    - type: syslog
      format: human
      level: warn
```

Defines the global section of the zrepl configuration. The default mirrors the
value included in the default zrepl configuration file.

```yaml
zrepl_config_jobs: []
```

Defines the jobs section of the zrepl configuration. Note that zrepl will fail
to start with the default value as no jobs are defined.

```yaml
zrepl_service_state: started
```

Sets the state of the service whenever the role is invoked (refer to the
`state` parameter of the service module for valid values).

```yaml
zrepl_service_enabled: true
```

Defines whether zrepl is started on boot.

## Dependencies

This role has no dependencies.

## Example Playbook

```yaml
- hosts: zfs_servers
  become: yes
  roles:
     - transitiv.zrepl
  vars:
    zrepl_config_jobs:
      - name: snap_tank
        type: snap
        filesystems: {
          "tank<": true,
        }
        snapshotting:
          type: periodic
          interval: 1h
          prefix: zrepl_
        pruning:
          keep:
            - type: last_n
              count: 24
```

## License

This role is available under the terms of the MIT license.

## Author Information

This role was created in 2021 by [Transitiv Technologies
Ltd](https://www.transitiv.co.uk).
