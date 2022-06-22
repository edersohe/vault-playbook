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
ansible-playbook -b -u vagrant -k --tags=common main.yml
ansible-playbook -u devops --tags=primary_cluster main.yml
```

### Useful commands

```
export VAULT_ADDR=$API_ADDR
export VAULT_TOKEN=$ROOT_TOKEN
export VAULT_SKIP_VERIFY=true
vault status
vault operator init -status
vault operator init -key-shares=3 -key-threshold=2 > tokens.txt
vault operator unseal $UNSEAL_KEY
vault operator raft list-peers
vault operator raft snapshot save backup.snap
vault operator raft snapshot restore -force backup.snap
``` 
