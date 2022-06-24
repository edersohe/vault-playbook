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

### TLS - Create example CA and and wilcard certiticate (home.local) for servers

```
openssl genrsa -des3 -out rootCA.key 4096
openssl req -x509 -new -nodes -key rootCA.key -sha256 -days 7300 -out rootCA.crt -subj "/C=MX/ST=CDMX/O=My Home Lab/CN=ca.home.local"
openssl x509 -in rootCA.crt -text -noout
openssl genrsa -out home.local.key 2048
openssl req -new -key home.local.key -out home.local.csr -subj "/C=MX/ST=CDMX/O=My Home Lab/CN=*.home.local"
openssl req -in home.local.csr -noout -text
openssl x509 -req -in home.local.csr -CA rootCA.crt -CAkey rootCA.key -CAcreateserial -out home.local.crt -days 3650 -sha256
openssl x509 -in home.local.crt -text -noout
```

> tls_name variable for inventory.ini is the name of server certificates without extension example home.local

> when tls_name is defined into inventory.ini 3 files are needed into files directory at root of this project level example: home.local.crt, home.local.key and rootCA.crt

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
vault operator members
vault operator raft snapshot save backup.snap
vault operator raft snapshot restore -force backup.snap
vault audit enable file file_path=/vault/vault-audit.log
vault audit list -detailed
vault monitor -log-level=trace
``` 

# References

* https://learn.hashicorp.com/tutorials/vault/raft-deployment-guide?in=vault/raft
* https://learn.hashicorp.com/tutorials/vault/raft-storage?in=vault/raft
* https://learn.hashicorp.com/tutorials/vault/troubleshooting-vault
* https://github.com/hashicorp/vault-selinux-policies
* https://gist.github.com/fntlnz/cf14feb5a46b2eda428e000157447309
* https://stackoverflow.com/questions/5051655/what-is-the-purpose-of-the-nodes-argument-in-openssl
