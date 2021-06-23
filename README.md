# Deploying Rails

I learnt a lot from the **[Efficient Rails DevOps](https://efficientrailsdevops.com/)** book by **[Michael Trojanek](https://relativkreativ.at/about)**. - Thank you!

## Prepare
- Install Virtualbox
- Install Vagrant
```bash
brew install vagrant
```

With Python
```bash
pip install ansible
pip install -r requirements.txt
```

## Generate password

**Important: the hash changes from OS to OS. Use the VM to create a hash.**

```bash
shh ansible@172.28.128.20
python -c 'import crypt,getpass,sys;print(crypt.crypt(getpass.getpass(), crypt.mksalt(crypt.METHOD_SHA512)))'
```

## Ansible Vault
```bash
ansible-vault create group_vars/all/vault
ansible-vault edit group_vars/all/vault

cat ~/.ssh/id_rsa.pub >> roles/user/files/ansible/authorized_keys
ansible-vault encrypt roles/user/files/ansible/*
```

## Vagrant

### Snapshots

* vagrant snapshot save [snapshot-name]
* vagrant snapshot restore [snapshot-name]
* vagrant snapshot delete [snapshot-name]
* vagrant snapshot list

## Services
```bash
ss -ltun
lsof -i :25
```

## How to use `app` user?
```bash
sudo su - app
```




