Merritt Ansible Service Restart
===============================

An Ansible role for doing rolling restarts of load-balanced Merritt applications.


### Prerequisites

This project depends on [uc3ops-ansible-inventory](https://github.com/cdluc3/uc3ops-ansible-inventory).
See the [README.md](https://github.com/cdluc3/uc3ops-ansible-inventory/blob/master/README.rst)
for complete instructions on the following:

  - [setup of python virtual env](https://github.com/CDLUC3/uc3ops-ansible-inventory#create-python-virual-environment-with-pyenv)
  - [install ansible with pip](https://github.com/CDLUC3/uc3ops-ansible-inventory#install-ansible-into-your-new-pyenv)
  - [Install and configure `uc3.aws_ec2` inventory](https://github.com/CDLUC3/uc3ops-ansible-inventory#configure-uc3aws_ec2-inventory)


After completing the above, validate you can run ansible inventory queries using `uc3.aws_ec2`:

    > ansible --list-hosts srvc_mrt | head



### Installation

Clone this git repo into your home directory on the command hosts:

    > git clone git@github.com:CDLUC3/mrt-ansible-service-restart.git


### Usage

#### Prepare your shell session

1. Be sure your python virtual environment where ansible is installed is activated:
       ~> pyenv version
       ~> pyenv global 3.9.16

2. To make use of predefined `fqsn` host groupings, source the file `etc/service_groups.rc` into
your shell session:

       > cd mrt-ansible-service-restart
       mrt-ansible-service-restart> . etc/service_groups.rc


#### Run ansible-playbook in noop mode (default)

This playbook requires a hostlist and a `service_name` defined at the command line.

The hostlist is calculated by `uc3.aws_ec2` inventory based on any of the set of service group
environment vars set in `etc/service_groups.rc`.

The `service_name` is the systemd service to restart.

    mrt-ansible-service-restart> ansible-playbook mrt_service_restart.yaml -l $uc3_mrt_ui_stg -e service_name=puma

    mrt-ansible-service-restart> ansible-playbook mrt_service_restart.yaml -l $uc3_mrt_inventory_prd -e service_name=mrt-inventory


#### Run for reals

Supply an additional commandline environment var (`exec=true`) to actully execute the restarts:

    mrt-ansible-service-restart> ansible-playbook mrt_service_restart.yaml -l $uc3_mrt_inventory_prd -e service_name=mrt-inventory -e exec=true

    mrt-ansible-service-restart> ansible-playbook mrt_service_restart.yaml -l $uc3_mrt_store_stg -e service_name=mrt-store -e exec=true



### TODO

- report which AZ is processing at each control loop iteration
- when running in noop, report targetgroup configs and systemctl status.
- when restarting a service not in an ALB, validate the service is running properly (how?)
- when restarting a ALB managed service, make sure there are at least two targets in the targetgroups before running in any AZ.








### What it does and how it does it

predefined `fqsn` host groupings



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


Notes
-----

I had problem running the 3.9.16 python virtual environment as uc3aws.  

First I had to install python with pip,
then I discovered I had not built the pyenv properly - various devel libs had been missing.


```

-bash-4.2$ pyenv versions
* system (set by /apps/uc3aws/.pyenv/version)
  2.7.18
  3.9.16
-bash-4.2$ py3
-bash-4.2$ pyenv versions
  system
  2.7.18
* 3.9.16 (set by PYENV_VERSION environment variable)

-bash-4.2$ pip list
Package         Version
--------------- --------
argcomplete     2.0.0
boto3           1.26.116
botocore        1.29.116
click           8.1.3
jmespath        1.0.1
pip             23.2.1
python-dateutil 2.8.2
PyYAML          6.0
s3transfer      0.6.0
setuptools      58.1.0
six             1.16.0
toml            0.10.2
urllib3         1.26.15
xmltodict       0.13.0
yq              3.1.0

-bash-4.2$ pip install ansible
Collecting ansible
[cut]
Successfully installed MarkupSafe-2.1.3 ansible-8.4.0 ansible-core-2.15.4 cffi-1.15.1 cryptography-41.0.4 importlib-resources-5.0.7 jinja2-3.1.2 packaging-23.1 pycparser-2.21 resolvelib-1.0.1


-bash-4.2$ pip list
Package             Version
------------------- --------
ansible             8.4.0
ansible-core        2.15.4
argcomplete         2.0.0
boto3               1.26.116
botocore            1.29.116
cffi                1.15.1
click               8.1.3
cryptography        41.0.4
importlib-resources 5.0.7
Jinja2              3.1.2
jmespath            1.0.1
MarkupSafe          2.1.3
packaging           23.1
pip                 23.2.1
pycparser           2.21
python-dateutil     2.8.2
PyYAML              6.0
resolvelib          1.0.1
s3transfer          0.6.0
setuptools          58.1.0
six                 1.16.0
toml                0.10.2
urllib3             1.26.15
xmltodict           0.13.0
yq                  3.1.0


-bash-4.2$ ansible-galaxy collections list
ERROR: No module named '_ctypes'


My python 3.9.16 build is broken.  I need to install devel packages and re-complile python

agould@uc3-aws2-ops:~/ansible/projects/uc3_patching> sudo yum install bzip2-devel ncurses-devel libffi-devel readline-devel

Installed:
  bzip2-devel.x86_64 0:1.0.6-13.amzn2.0.3               libffi-devel.x86_64 0:3.0.13-18.amzn2.0.2      
  ncurses-devel.x86_64 0:6.0-8.20170212.amzn2.1.5       readline-devel.x86_64 0:6.2-10.amzn2.0.2       

Dependency Installed:
  ncurses-c++-libs.x86_64 0:6.0-8.20170212.amzn2.1.5                                                    

-bash-4.2$ pyenv install -f 3.9.16
Installing Python-3.9.16...
Installed Python-3.9.16 to /apps/uc3aws/.pyenv/versions/3.9.16

-bash-4.2$ ansible-galaxy collection list | grep community.aws
community.aws                 6.3.0  

```
