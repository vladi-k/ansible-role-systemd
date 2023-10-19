ansible-role-systemd
====

Manage systemd units.

Requirements
------------

* RHEL7+

Role Variables
--------------

* `systemd_scripts` - list of systemd scripts, example:

```yaml
systemd_scripts:
  - dest: /usr/local/bin/my-script
    content: |
      #!/bin/bash
      echo "it works"
```

* `systemd_units` - list of systemd units, example:

```yaml
systemd_units:
  - name: my-custom.service
    content: |
      [Unit]
      Description=My custom service (ansible managed)
      Wants=my-custom.timer

      [Service]
      Type=oneshot
      ExecStart=/usr/local/bin/my-script

      [Install]
      WantedBy=default.target
    enabled: true
    state: started
  - name: my-custom.timer
    content: |
      [Unit]
      Description=My custom timer (ansible managed)
      Requires=my-custom.service

      [Timer]
      Unit=my-custom.service
      OnCalendar=*:0/10

      [Install]
      WantedBy=timers.target
```

Dependencies
------------

Collections:

* `ansible.builtin`

Example Playbook
----------------

```
- hosts: my_servers
  # see example in section above
  # vars:
  roles:
    - ansible-role-systemd
```

License
-------

GPLv3

Author Information
------------------

Vladimir Vasilev (@vladi-k)
