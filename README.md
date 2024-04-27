# ansible-role-firewalld

This role allows adding and removing most types of firewalld rules from the default zone

 - https://firewalld.org/


## Task Configuration

```
- name: Test adding and removeing services etc
  hosts: test
  become: true
  roles:
    - role: firewalld
      firewalld_ipset_add:
        - name: peers
          ips:
            - 207.188.6.74
            - 207.188.6.12
            - 207.188.6.49

      firewalld_add:
        - name: public
          masquerade: false
          forward: true
          services:
            - http
            - https
            - ssh
          ports:
            - 53/tcp
            - 53/udp
            - 67/udp
            - 547/udp
          forwards:
            - port: 443
              proto: udp
              to: 51820
        - name: ftl
          interfaces:
            - lo
          ports:
            - 4711/tcp

      firewalld_remove:
        - name: public
          masquerade: true
          services:
            - http
            - https
```


## Deployment and Removal

Deploy

```
ansible-playbook -i hosts site.yml --tags=firewalld --limit=somehost
```

Remove

```
ansible-playbook -i hosts site.yml --tags=firewalld --extra-vars "firewall_action=remove" --limit=somehost
```
