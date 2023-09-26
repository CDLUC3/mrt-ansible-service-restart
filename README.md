Merritt Ansible Service Restart
===============================

An Ansible role for doing rolling restarts of load-balanced Merritt applications.


### Prerequisites

This project depends on [uc3ops-ansible-inventory](https://github.com/cdluc3/uc3ops-ansible-inventory).
See the [README.md](https://github.com/cdluc3/uc3ops-ansible-inventory/blob/master/README.rst)
for complete instructions on the following:

  - [setup of python virtual env](https://github.com/CDLUC3/uc3ops-ansible-inventory#create-python-virual-environment-with-pyenv)
  - [install ansible with pip](https://github.com/CDLUC3/uc3ops-ansible-inventory#install-ansible-into-your-new-pyenv)
  - [create your `ansible.cfg` file](https://github.com/CDLUC3/uc3ops-ansible-inventory#configure-uc3aws_ec2-inventory)














### Installation as `uc3aws` role user

To clone this github repo as `uc3aws` role user on the UC3 "ops" box `uc3-aws2-ops.cdlib.org`,
I had to configure **deployment keys** in Github.

```
-bash-4.2$ ssh-keygen
Generating public/private rsa key pair.
Enter file in which to save the key (/apps/uc3aws/.ssh/id_rsa): /apps/uc3aws/.ssh/id_rsa.uc3ops-ansible-inventory
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /apps/uc3aws/.ssh/id_rsa.uc3ops-ansible-inventory.
Your public key has been saved in /apps/uc3aws/.ssh/id_rsa.uc3ops-ansible-inventory.pub.
The key fingerprint is:
SHA256:uHRAkfXpToqLvxKFJHnlOjSyFvm3q7NAblDy9oWUpuI uc3aws@uc3-aws2-ops
```

```
-bash-4.2$ cd .ssh
-bash-4.2$ ll
total 12
-rw------- 1 uc3aws uc3aws 1675 Sep 25 16:46 id_rsa.uc3ops-ansible-inventory
-rw-r--r-- 1 uc3aws uc3aws  401 Sep 25 16:46 id_rsa.uc3ops-ansible-inventory.pub
-rw-r--r-- 1 uc3aws uc3aws  444 Jul 14  2016 known_hosts
-bash-4.2$ vi ~/.ssh/config
-bash-4.2$ chmod 700 ~/.ssh/config
-bash-4.2$ ls -ld ~/.ssh/config 
-rwx------ 1 uc3aws uc3aws 163 Sep 25 16:50 /apps/uc3aws/.ssh/config
ash-4.2$ cat ~/.ssh/config
HashKnownHosts no
StrictHostKeyChecking no

Host github-uc3ops-ansible-inventory
    HostName github.com
    IdentityFile ~/.ssh/id_rsa.uc3ops-ansible-inventory

```

```
-bash-4.2$ cd /apps/uc3aws/ansible
-bash-4.2$ git remote set-url origin git@github-uc3ops-ansible-inventory:CDLUC3/uc3ops-ansible-inventory.git
-bash-4.2$ git remote show origin 
Warning: Permanently added 'github.com,192.30.255.113' (ECDSA) to the list of known hosts.
* remote origin
  Fetch URL: git@github-uc3ops-ansible-inventory:CDLUC3/uc3ops-ansible-inventory.git
  Push  URL: git@github-uc3ops-ansible-inventory:CDLUC3/uc3ops-ansible-inventory.git
  HEAD branch: master
  Remote branch:
    master tracked
  Local branch configured for 'git pull':
    master merges with remote master
  Local ref configured for 'git push':
    master pushes to master (local out of date)
-bash-4.2$ git pull
```

