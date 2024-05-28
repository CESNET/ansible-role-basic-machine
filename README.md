# ansible-role-basic-machine
Ansible role for basic common setup of machines.

It just imports the following roles:
- [cesnet.work_env](../cesnet.work_env/README.md)
- [cesnet.ntp](../cesnet.ntp/README.md)
- [cesnet.metacentrum_monitoring](../cesnet.metacentrum_monitoring/README.md)
- [cesnet.unattended-upgrades](../cesnet.unattended_upgrades/README.md)
- [cesnet.yubikeys](../cesnet.yubikeys/README.md)
- [cesnet.firewall](../cesnet.firewall/README.md)

Role Variables
--------------
The role has no own variables. However, the included roles do have variables. They have sensible defaults,
but the default firewall rules allow only ssh protocol and the default list of users is empty, 
so you likely want to change those. 

At least the variable **root_email_address** must be defined to contain the email address to which reports
from unattended upgrades will be sent.

Example playbook:
----------------
```yaml
- name: "run role cesnet.basic_machine on a machine"
  hosts: all
  remote_user: root
  roles:
    - role: cesnet.basic_machine
      vars:
        root_email_address: makub@ics.muni.cz
        unattended_upgrades_automatic_reboot: true
        firewall_open_tcp_ports:
          - { port: 80, comment: "accept http from everywhere" }
          - { port: 443, comment: "accept https from everywhere" }
        yubikey_users: "{{ perun_yubikey_users }}"
        yubikey_lognames: [ 'makub', 'zlamalp' ]
```