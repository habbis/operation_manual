# ansible tips and tricks 

## when using ansible 2.10 and later diffrent modules is located in collection so here is few that is nice to have.


posix
```
ansible-galaxy collection install ansible.posix
```

freeipa 
```
ansible-galaxy collection install freeipa.ansible_freeipa
```

general
```
ansible-galaxy collection install community.general
```
## ansible tuning 

You can add pipelining and forks to speed up ansible so it can execute playbooks in paralell.

stdout_callback option is more for debuging.

Add this to your local ansible.cfg or in the global /etc/ansible/ansible.cfg you may need to create it.

```
[defaults]
stdout_callback = yaml
host_key_checking = False
pipelining=True
forks=300
deprecation_warnings=False
```
