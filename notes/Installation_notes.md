

### Installation as `loy` usr

Set up pyenv

```
curl https://pyenv.run | bash
cat << \EOF >> ~/.bash_profile

# pyenv stuff
#
export PATH="/apps/uc3aws/.pyenv/bin:$PATH"
eval "$(pyenv init -)" 
eval "$(pyenv virtualenv-init -)" 
EOF
source ~/.bash_profile
which pyenv
pyenv versions
pyenv install 3.9.16
pyenv global 3.9.16
```

Install ansible
```
pip install -U pip
pip install ansible
```

Install/configure `uc3.aws_ec2` inventory

```
git clone 

 


### Installation as `uc3aws` role user


To clone this github repo as `uc3aws` role user on the UC3 "ops" box `uc3-aws2-ops.cdlib.org`,
I had to configure **deployment keys** in Github.

```
-bash-4.2$ ssh-keygen -f /apps/uc3aws/.ssh/id_rsa.mrt-ansible-service-restart
Generating public/private rsa key pair.
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /apps/uc3aws/.ssh/id_rsa.mrt-ansible-service-restart.
Your public key has been saved in /apps/uc3aws/.ssh/id_rsa.mrt-ansible-service-restart.pub.
The key fingerprint is:
SHA256:M4VJ/eNpxU4U+1X7WAK+XJGp16tYvmnPEZz1LKAANiI uc3aws@uc3-aws2-ops
```

(Now install the new ssh public key as a deployment key in the github repository)

```
-bash-4.2$ vi .ssh/config 
ash-4.2$ cat .ssh/config
HashKnownHosts no
StrictHostKeyChecking no

Host github-mrt-ansible-service-restart
    HostName github.com
    IdentityFile ~/.ssh/id_rsa.uc3ops-ansible-inventory

-bash-4.2$ chmod 700 ~/.ssh/config
-bash-4.2$ cd ansible/projects/
-bash-4.2$ git clone git@github-uc3ops-ansible-inventory:CDLUC3/mrt-ansible-service-restart.git
Cloning into 'mrt-ansible-service-restart'...
```


This is for uc3ops-ansible-inventory

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

Host github-mrt-ansible-service-restart
    HostName github.com
    IdentityFile ~/.ssh/id_rsa.uc3ops-ansible-inventory

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

```
-bash-4.2$ pwd
/apps/uc3aws/ansible
-bash-4.2$ ll
total 40
-rw-rw-r-- 1 uc3aws uc3aws  837 Sep 25 16:59 ansible.cfg.example
drwxrwxr-x 2 uc3aws uc3aws 4096 Sep 25 16:59 etc
drwxrwxr-x 2 uc3aws uc3aws 4096 Sep  4  2020 group_vars
drwxrwxr-x 2 uc3aws uc3aws 4096 Sep  4  2020 host_vars
drwxrwxr-x 2 uc3aws uc3aws 4096 Sep 25 16:59 inventory
drwxrwxr-x 2 uc3aws uc3aws 4096 Sep 25 16:59 notes
drwxrwxr-x 2 uc3aws uc3aws 4096 Sep 25 16:59 playbooks
drwxrwxr-x 2 uc3aws uc3aws 4096 Oct  5  2020 projects
-rw-rw-r-- 1 uc3aws uc3aws 6296 Sep 25 16:59 README.md
-bash-4.2$ cp ansible.cfg.example ~/.ansible.cfg
-bash-4.2$ ansible --list-hosts all
  hosts (84):
    uc3-etdx2-prd
    uc3-mrtzk04x2-prd
    uc3-wasredirectx2-prd
    uc3-etdx2-stg
    uc3-uc3ops01x2-dev
    uc3-mrtsandbox2-stg
    uc3-mrtzk05x2-prd
    uc3-dryadui02x2-stg
 [cut]
```


