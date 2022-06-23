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
export VAULT_FORMAT=yaml
vault status
vault operator init -status
vault operator init -key-shares=3 -key-threshold=2 > tokens.yml
vault operator unseal $UNSEAL_KEY
vault operator raft list-peers
vault operator raft snapshot save backup.snap
vault operator raft snapshot restore -force backup.snap
vault audit enable file file_path=/vault/vault-audit.log
vault audit list -detailed
``` 

# References

* https://learn.hashicorp.com/tutorials/vault/raft-deployment-guide?in=vault/raft
* https://learn.hashicorp.com/tutorials/vault/raft-storage?in=vault/raft
* https://learn.hashicorp.com/tutorials/vault/troubleshooting-vault
* https://github.com/hashicorp/vault-selinux-policies
