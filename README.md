### Vagrant (Home Lab Servers)

```
vagrant up
```

### Ansible denpendencies

```
apt install sshpass ; #debian/ubuntu
dnf install sshpass ; #RHEL/Rocky/Alma/Oracle/Fedora
pipx install ansible-core
pipx inject --include-deps --include-apps ansible-core jmespath
ansible-galaxy install -r requirements.yml
```

### Vault Binary

> put the vault binary file into roles/vault/files named just vault without prefixes, suffixes, version or extension.

### Enable TLS communication for vault

> tls_name variable group(cluster) into inventory.ini enable TLS communication for vault

> tls_name variable has to be equal to the name of server certificates without extension, ex."tls_name=home.local" when home.local.crt and home.local.key are the servers certificates

> when tls_name is defined 3 files are needed into directory roles/vault/files ex. for "tls_name=home.local" the files home.local.crt, home.local.key and rootCA.crt are needed

> for convention CA certificate always be named rootCA.crt

#### Example for certificates generation

```
openssl genrsa -des3 -out rootCA.key 4096
openssl req -x509 -new -nodes -key rootCA.key -sha256 -days 7300 -out rootCA.crt -subj "/C=MX/ST=CDMX/O=My Home Lab/CN=ca.home.local"
openssl x509 -in rootCA.crt -text -noout
openssl genrsa -out home.local.key 2048
openssl req -new -key home.local.key -out home.local.csr -subj "/C=MX/ST=CDMX/O=My Home Lab/CN=*.home.local"
openssl req -in home.local.csr -noout -text
openssl x509 -req -in home.local.csr -CA rootCA.crt -CAkey rootCA.key -CAcreateserial -out home.local.crt -days 3650 -extfile <(printf "subjectAltName=DNS:*.home.local")
openssl x509 -in home.local.crt -text -noout
```

### Run playbooks

```
ansible-playbook -b -u vagrant -k --tags=common main.yml
ansible-playbook -u devops --tags=autounseal_cluster main.yml
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
vault operator init
vault operator unseal $UNSEAL_KEY
vault secrets enable transit
vault write -f transit/keys/autounseal
vault policy write autounseal auntounseal.hcl
vault token create -policy=autounseal
vault operator raft list-peers
vault operator members
vault operator raft snapshot save backup.snap
vault operator raft snapshot restore -force backup.snap
vault audit enable file file_path=/vault/vault-audit.log
vault audit list -detailed
vault audit enable file file_path=/workarea/vault/log/audit.log
vault monitor -log-level=trace
``` 

# References

* https://learn.hashicorp.com/tutorials/vault/raft-deployment-guide?in=vault/raft
* https://learn.hashicorp.com/tutorials/vault/raft-storage?in=vault/raft
* https://learn.hashicorp.com/tutorials/vault/troubleshooting-vault
* https://github.com/hashicorp/vault-selinux-policies
* https://gist.github.com/fntlnz/cf14feb5a46b2eda428e000157447309
* https://stackoverflow.com/questions/5051655/what-is-the-purpose-of-the-nodes-argument-in-openssl
* https://learn.hashicorp.com/tutorials/vault/autounseal-transit?in=vault/auto-unseal
