## Create `Vagrantfile` and Run `ssh-keygen`
```
mkdir .ssh
cd .ssh
ssh-keygen
/Users/sai.linnthu/pov-vagrant/03-ansible/.ssh/id_ed25519
```

### To clone a specific branch
```
git clone -b 03-ansible --single-branch git@gitlab.com:sailinnthu/pov-vagrant.git 03-ansible
```
# OPTION 1 - Enable Mounting of local directory into VM
```
config.vm.synced_folder '.', '/home/vagrant'
```

### Run `vagrant up` and check your `~/.ssh/authorized_keys`
* ONLY the public key you generated will be inserted into `~/.ssh/authorized_keys`
```
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAICzNWyghSpTo6UX67sCjh4MhYRbswaQqPfjkPZP4wWh+ sai.linnthu@sai.linnthu-ABCDEFGHAJ
```

### Check `vagrant ssh-config`
```
$ vagrant ssh-config
Host ansible
  HostName 127.0.0.1
  User vagrant
  Port 2222
  UserKnownHostsFile /dev/null
  StrictHostKeyChecking no
  PasswordAuthentication no
  IdentityFile /Users/sai.linnthu/pov-vagrant/03-ansible/.vagrant/machines/ansible/vmware_desktop/private_key
  IdentitiesOnly yes
  LogLevel FATAL
  PubkeyAcceptedKeyTypes +ssh-rsa
  HostKeyAlgorithms +ssh-rsa
```

### When you run `vagrant ssh`, it will connect to `vagrant@127.0.0.1` and you `HAVE TO TYPE` password
```
$ vagrant ssh
vagrant@127.0.0.1's password:
```

### Before you run the next step, make sure you setup `~/.ssh/config` as below under your `$HOME`
```
$ cat ~/.ssh/config 

Host 192.168.56.99
  # IdentityFile should point to the *private key* of the public key specified
  # by PUBLIC_KEY_PATH in the Vagrantfile
  IdentityFile /Users/sai.linnthu/pov-vagrant/03-ansible/.ssh/id_ed25519
  User vagrant
  UserKnownHostsFile /dev/null
  StrictHostKeyChecking no
```

### When you access as `ssh vagrant@192.168.56.99`, you DO NOT need to type password

### When you access as `ssh 192.168.56.99`, you DO NOT need to type password

---
---

# OPTION 2 - Disable Mounting of local directory into VM
```
config.vm.synced_folder '.', '/home/vagrant', disabled: true
```

### Run `vagrant up` and check your `~/.ssh/authorized_keys`
* The public key you generated, the public keys vagrant generated automatically are going to be inserted into `~/.ssh/authorized_keys` as below. You can verify later once VM is up.
```
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIN1YdxBpNlzxDqfJyw/QKow1F+wvG9hXGoqiysfJOn5Y vagrant insecure public key
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQC6zBJIhd8ipa5Gf+y3wwvomzzcFYCstpv3YzPqehPoTWaRuyblfT4fxUfibWkSQkzvCbX2HUvjyF7BmIiOcUzlFyd2bv/JE2+luijZyWtlNFsAs574hZJIoMxMU8DMOFLTaVvUbvis+loTavyOc3hbjz2madEI4HnGJ+W++NjMV2bPzXfnmcANHjr1p2/J3wkek5Bfhu+uLeF7Moh7el2lboA+BQmP/B++dK9MbT745i2eKN+Gyylr8MW6Qd4IoKCdOy3xsLyjib1yd2uqQocqjmEVfmOqPkY0W1alN1phnZ2GKeidCiN+xpZttqcR+Wqu+U4WJECJulk2WVNM6IAz vagrant
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAICzNWyghSpTo6UX67sCjh4MhYRbswaQqPfjkPZP4wWh+ sai.linnthu@sai.linnthu-ABCDEFGHAJ
```

### Check `vagrant ssh-config`
```
$ vagrant ssh-config
Host ansible
  HostName 127.0.0.1
  User vagrant
  Port 2222
  UserKnownHostsFile /dev/null
  StrictHostKeyChecking no
  PasswordAuthentication no
  IdentityFile /Users/sai.linnthu/pov-vagrant/03-ansible/.vagrant/machines/ansible/vmware_desktop/private_key
  IdentitiesOnly yes
  LogLevel FATAL
  PubkeyAcceptedKeyTypes +ssh-rsa
  HostKeyAlgorithms +ssh-rsa
```

### When you run `vagrant ssh`, you DO NOT need to type password
```
$ vagrant ssh
vagrant@127.0.0.1's password:
```

### Before you run the next step, make sure you setup `~/.ssh/config` as below under your `$HOME`
```
$ cat ~/.ssh/config 

Host 192.168.56.99
  # IdentityFile should point to the *private key* of the public key specified
  # by PUBLIC_KEY_PATH in the Vagrantfile
  IdentityFile /Users/sai.linnthu/pov-vagrant/00-vagrant-getstarted/.ssh/id_ed25519
  User vagrant
  UserKnownHostsFile /dev/null
  StrictHostKeyChecking no
```

### When you access as `ssh vagrant@192.168.56.99`, you DO NOT need to type password

### When you access as `ssh 192.168.56.99`, you DO NOT need to type password

## Copying Files and Folders from Host to VM
* Because you disable Mounting of local directory into VM, you need to use `scp` to transfer files and folders from Host Machine to VM.

* Copy `README.md` file from Host Machine to VM using `scp`
```
$ scp README.md 192.168.56.99:/home/vagrant/
Warning: Permanently added '192.168.56.99' (ED25519) to the list of known hosts.
README.md                                                                                                                                                                                                    100% 3773    10.8MB/s   00:00
```
* Verify
```
$ ssh 192.168.56.99

vagrant@ansible:~$ ls -la | grep README
-rw-r--r-- 1 vagrant vagrant 13262 Jul 16 13:59 README.md
```

* Create test folders and files on Host Machine as below
```
$ ls -la kubernetes/
total 0
drwxr-xr-x  5 sai.linnthu  staff  160 Jul 16 21:40 .
drwxr-xr-x  7 sai.linnthu  staff  224 Jul 16 21:40 ..
-rw-r--r--  1 sai.linnthu  staff    0 Jul 16 21:40 deployment.yaml
-rw-r--r--  1 sai.linnthu  staff    0 Jul 16 21:40 service.yaml
drwxr-xr-x  4 sai.linnthu  staff  128 Jul 16 21:40 terraform

$ ls -la kubernetes/terraform/
total 0
drwxr-xr-x  4 sai.linnthu  staff  128 Jul 16 21:40 .
drwxr-xr-x  5 sai.linnthu  staff  160 Jul 16 21:40 ..
-rw-r--r--  1 sai.linnthu  staff    0 Jul 16 21:40 main.tf
-rw-r--r--  1 sai.linnthu  staff    0 Jul 16 21:40 variables.tf
```

* Copy the folders from Host Machine to VM using `scp -r`
```
$ scp -r kubernetes/ 192.168.56.99:/home/vagrant/
Warning: Permanently added '192.168.56.99' (ED25519) to the list of known hosts.
main.tf                                                                                                                                                                                                      100%    0     0.0KB/s   00:00    
variables.tf                                                                                                                                                                                                 100%    0     0.0KB/s   00:00    
deployment.yaml                                                                                                                                                                                              100%    0     0.0KB/s   00:00    
service.yaml                                                                                                                                                                                                 100%    0     0.0KB/s   00:00 
```
* Verify
```
$ ssh 192.168.56.99

vagrant@ansible:~$ ls -la kubernetes/
total 12
drwxr-xr-x 3 vagrant vagrant 4096 Jul 16 14:00 .
drwxr-x--- 5 vagrant vagrant 4096 Jul 16 14:00 ..
-rw-r--r-- 1 vagrant vagrant    0 Jul 16 14:00 deployment.yaml
-rw-r--r-- 1 vagrant vagrant    0 Jul 16 14:00 service.yaml
drwxr-xr-x 2 vagrant vagrant 4096 Jul 16 14:00 terraform

vagrant@ansible:~$ ls -la kubernetes/terraform/
total 8
drwxr-xr-x 2 vagrant vagrant 4096 Jul 16 14:00 .
drwxr-xr-x 3 vagrant vagrant 4096 Jul 16 14:00 ..
-rw-r--r-- 1 vagrant vagrant    0 Jul 16 14:00 main.tf
-rw-r--r-- 1 vagrant vagrant    0 Jul 16 14:00 variables.tf
```

## Verify `sshd_config`
```
vagrant@ansible:~$ cat /etc/ssh/sshd_config | grep Pub
#PubkeyAuthentication yes

vagrant@ansible:~$ cat /etc/ssh/sshd_config | grep Author
#AuthorizedKeysFile     .ssh/authorized_keys .ssh/authorized_keys2
#AuthorizedPrincipalsFile none
#AuthorizedKeysCommand none
#AuthorizedKeysCommandUser nobody

vagrant@ansible:~$ cat /etc/ssh/sshd_config | grep Password
#PasswordAuthentication yes
#PermitEmptyPasswords no
# PasswordAuthentication.  Depending on your PAM configuration,
# PAM authentication, then enable this but set PasswordAuthentication

```