### Vagrant

```
vagrant up
```


### Ansible denpendencies

```
apt install sshpass
dnf install sshpass
pipx install ansible-core
pipx inject --include-deps --include-apps ansible-core jmespath
ansible-galaxy install -r requirements.yml
```

### Run playbooks

```
ansible-playbook -b -u vagrant -k  --tags=common main.yml
```
