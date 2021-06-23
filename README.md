# Deploying Rails

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




