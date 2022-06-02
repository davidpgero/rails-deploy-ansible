# Deploying Rails

I learnt a lot from
* The **[Efficient Rails DevOps](https://efficientrailsdevops.com/)** book by **[Michael Trojanek](https://relativkreativ.at/about)**.
* The **[Mastering Ansible](https://www.packtpub.com/product/mastering-ansible/9781784395483)** book by **Jesse Keating** and **James Freeman**.

- Thank you!

## Prepare
Instead of Vagrant with Virtualbox, we're going to use [Multipass](https://multipass.run/) as our primary VM. 
- Install Multipass

With Python
```bash
pip install ansible
pip install -r requirements.txt
ansible-galaxy install -r requirements.yml
```

## Run
```bash
# launch multipass
multipass launch --cloud-init cloud-init.yaml --name 'PICK_YOUR_NAME' 20.04
# run the provision
ansible-playbook site.yml --verbose
```

## Generate password

**Important: the hash can change from OS to OS. Use the VM to create a hash.**

```bash
ssh ubuntu@192.168.64.6
python3 -c 'import crypt,getpass,sys;print(crypt.crypt(getpass.getpass(), crypt.mksalt(crypt.METHOD_SHA512)))'
```

## Ansible Vault
```bash
ansible-vault create group_vars/all/vault
ansible-vault edit group_vars/all/vault

cat ~/.ssh/id_rsa.pub >> roles/user/files/ansible/authorized_keys
ansible-vault encrypt roles/user/files/ansible/*

# replace keys
ansible-vault rekey group_vars/*/vault roles/user/files/*/*
ansible-vault edit group_vars/all/vault
```

**If you totally forget your password...** We can't get them back, but we can recreate the files.

- Find the vault files `grep '$ANSIBLE_VAULT' . -r -l`
- Store the list somewhere so you can go one by one
- Remove the file and encrypt it again `ansible-vault encrypt YOUR_FILE`
- Whenever we use enrcyped variables, you can find the name of the value by `vault_` prefix
    - Open a new vault file, add the variable and encrypt the file


## Multipass

[Multipass](https://multipass.run/) as a primary VM.

You can set your SSH public key in the `cloud-init.yml` file. 
After you set up your SSH key, you can ssh in `ssh ubuntu@VM_IP_ADDRESS`.
Get your `VM_IP_ADDRESS` by `multipass info --all`
```yml
ssh_authorized_keys:
  - YOUR_SSH_PUBLIC_KEY
```

Useful for set up SSH key in your `~/.ssh/config`. 
```bash
Host VM_IP_ADDRESS
  IdentityFile ~/.ssh/YOUR_PRIVATE_KEY
  User ubuntu
  ForwardAgent yes
```

Useful commands are:
```bash
# Launch a new VM
multipass launch --cloud-init cloud-init.yml --name 'YOUR_VM_NAME' 20.04
# Get info about your VM
multipass info YOUR_VM_NAME
```

## Services
```bash
ss -ltun
lsof -i :25
```

## How to use `app` user?
```bash
sudo su - app
```




