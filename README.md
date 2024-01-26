Merritt Ansible Service Manager
===============================

This project consists of 2 ansible playbooks for managing Merritt services:

- [**mrt-service-restart.yaml**](#playbook-mrt-service-restart)
- [**report-deployed-webapp.yaml**](#playbook-report-deployed-webapp.yaml)


## Playbook mrt-service-restart

An Ansible role for doing rolling deployments and restarts of load-balanced Merritt applications.

### What it does

The `mrt-service-restart.yaml` playbook does the following:
- Determine to which ALB TargetGroups the instance is registered (if any)
- Cycle through each AWS Availablity zone and run the following tasks:
  - Handle ALB target de-registration
  - Run Puppet
  - Deploy Tomcat webapp (when `-e deploy=true` option is set) 
  - Restart service
  - Report deployed build tag (tomcat services)
  - Handle ALB target re-registration

If any ansible task fails, the playbook halts, and no pending tasks are run.  This
is to prevent a situation where a bad deployment or restart is propogated across
all nodes in a ALB TargetGroup.  Once the operator has cleared the failure, the playbook
can be restarted.


### Command syntax and option summary

```
ansible-playbook mrt-service-restart.yaml -l <host_list> -e service_name=<string>
```

#### Arguments

**host_list** 
> Required.  Ansible host inventory.  This is specified as a shell var.
> See `etc/service_groups.rc`.

#### Extra vars

Ansible extra vars are specified on the commandline with the `-e` option, as in `-e exec=true`.

**service_name**
> Required.  Name of the systemd service to be restarted. 
> See `group_vars/all` for list of allowed values.

**exec**
> Boolean. Set to true to effect changes.
>
> Default: `false`

**run_puppet**
> Boolean. Whether to run `puppet apply` prior to deployment/restart.
>
> Default: `true`

**deploy**
> Boolean. Set to true to deploy a Merritt java webapp into a running Tomcat instance.
> The version to deploy is set in local file `$APP_HOME/bin/cap_deploy.sh`.
>
> Default: `false`

**restart**
> Boolean. Whether to restart the Merritt service in `systemd`.
>
> Default: `true`



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

    > ansible-playbook mrt_service_restart.yaml -l $uc3_mrt_ui_stg -e service_name=puma

    > ansible-playbook mrt_service_restart.yaml -l $uc3_mrt_inventory_prd -e service_name=mrt-inventory


#### Run for reals

Supply extra var `exec=true` at the commandline to actully execute the restarts:

    > ansible-playbook mrt_service_restart.yaml -l $uc3_mrt_inventory_prd -e service_name=mrt-inventory -e exec=true

    > ansible-playbook mrt_service_restart.yaml -l $uc3_mrt_store_stg -e service_name=mrt-store -e exec=true


#### Run a deployment

Supply extra var `deploy=true` at the commandline to initiate deployment of java webeapp:

    > ansible-playbook mrt_service_restart.yaml -l $uc3_mrt_inventory_prd -e service_name=mrt-inventory -e deploy=true

    > ansible-playbook mrt_service_restart.yaml -l $uc3_mrt_inventory_prd -e service_name=mrt-inventory -e deploy=true -e exec=true


### Command lines for Cut-n-Paste

#### Running restarts for all services

Stage:
```
. etc/service_groups.rc

ansible-playbook mrt_service_restart.yaml -l $uc3_mrt_access_stg    -e service_name=mrt-access    -e exec=true
ansible-playbook mrt_service_restart.yaml -l $uc3_mrt_audit_stg     -e service_name=mrt-audit     -e exec=true
ansible-playbook mrt_service_restart.yaml -l $uc3_mrt_ingest_stg    -e service_name=mrt-ingest    -e exec=true
ansible-playbook mrt_service_restart.yaml -l $uc3_mrt_inventory_stg -e service_name=mrt-inventory -e exec=true
ansible-playbook mrt_service_restart.yaml -l $uc3_mrt_replic_stg    -e service_name=mrt-replic    -e exec=true
ansible-playbook mrt_service_restart.yaml -l $uc3_mrt_store_stg     -e service_name=mrt-store     -e exec=true
ansible-playbook mrt_service_restart.yaml -l $uc3_mrt_ui_stg        -e service_name=puma          -e exec=true
ansible-playbook mrt_service_restart.yaml -l $uc3_mrt_ldap_stg      -e service_name=ldap          -e exec=true
```

Production:
```
. etc/service_groups.rc

ansible-playbook mrt_service_restart.yaml -l $uc3_mrt_access_prd    -e service_name=mrt-access    -e exec=true
ansible-playbook mrt_service_restart.yaml -l $uc3_mrt_audit_prd     -e service_name=mrt-audit     -e exec=true
ansible-playbook mrt_service_restart.yaml -l $uc3_mrt_ingest_prd    -e service_name=mrt-ingest    -e exec=true
ansible-playbook mrt_service_restart.yaml -l $uc3_mrt_inventory_prd -e service_name=mrt-inventory -e exec=true
ansible-playbook mrt_service_restart.yaml -l $uc3_mrt_replic_prd    -e service_name=mrt-replic    -e exec=true
ansible-playbook mrt_service_restart.yaml -l $uc3_mrt_store_prd     -e service_name=mrt-store     -e exec=true
ansible-playbook mrt_service_restart.yaml -l $uc3_mrt_ui_prd        -e service_name=puma          -e exec=true
ansible-playbook mrt_service_restart.yaml -l $uc3_mrt_ldap_prd      -e service_name=ldap          -e exec=true
```


#### Running deployment for Tomcat services

Stage:
```
. etc/service_groups.rc

ansible-playbook mrt_service_restart.yaml -l $uc3_mrt_access_stg    -e service_name=mrt-access    -e deploy=true -e exec=true
ansible-playbook mrt_service_restart.yaml -l $uc3_mrt_audit_stg     -e service_name=mrt-audit     -e deploy=true -e exec=true
ansible-playbook mrt_service_restart.yaml -l $uc3_mrt_ingest_stg    -e service_name=mrt-ingest    -e deploy=true -e exec=true
ansible-playbook mrt_service_restart.yaml -l $uc3_mrt_inventory_stg -e service_name=mrt-inventory -e deploy=true -e exec=true
ansible-playbook mrt_service_restart.yaml -l $uc3_mrt_replic_stg    -e service_name=mrt-replic    -e deploy=true -e exec=true
ansible-playbook mrt_service_restart.yaml -l $uc3_mrt_store_stg -    e service_name=mrt-store     -e deploy=true -e exec=true
```

Production:
```
. etc/service_groups.rc

ansible-playbook mrt_service_restart.yaml -l $uc3_mrt_access_prd    -e service_name=mrt-access    -e deploy=true -e exec=true
ansible-playbook mrt_service_restart.yaml -l $uc3_mrt_audit_prd     -e service_name=mrt-audit     -e deploy=true -e exec=true
ansible-playbook mrt_service_restart.yaml -l $uc3_mrt_ingest_prd    -e service_name=mrt-ingest    -e deploy=true -e exec=true
ansible-playbook mrt_service_restart.yaml -l $uc3_mrt_inventory_prd -e service_name=mrt-inventory -e deploy=true -e exec=true
ansible-playbook mrt_service_restart.yaml -l $uc3_mrt_replic_prd    -e service_name=mrt-replic    -e deploy=true -e exec=true
ansible-playbook mrt_service_restart.yaml -l $uc3_mrt_store_prd -    e service_name=mrt-store     -e deploy=true -e exec=true
```
----------------------------------------------------



## Playbook report-deployed-webapp

This playbook reports both the configured version of the webapp build tag and
the currently deployed build tag.  It does this by grepping for
`MERRITT_SERVICE_RELEASE` in `~/bin/cap_deploy.sh`, and then downloading
`build.content.txt` from the webapp running on the localhost.

Usage example:
```
. etc/service_groups.rc
export ANSIBLE_STDOUT_CALLBACK=yaml
ansible-playbook report-deployed-webapp.yaml -l $uc3_mrt_audit_prd -e service_name=mrt-audit
```


### Cut-n-Paste commands to build a report on all services

```
. etc/service_groups.rc
export ANSIBLE_STDOUT_CALLBACK=yaml
OUTFILE=tmp/merritt_deployments.$(date +"%Y%m%d")

ansible-playbook report-deployed-webapp.yaml -l $uc3_mrt_access_stg -e service_name=mrt-access >> $OUTFILE
ansible-playbook report-deployed-webapp.yaml -l $uc3_mrt_access_prd -e service_name=mrt-access >> $OUTFILE

ansible-playbook report-deployed-webapp.yaml -l $uc3_mrt_audit_stg -e service_name=mrt-audit >> $OUTFILE
ansible-playbook report-deployed-webapp.yaml -l $uc3_mrt_audit_prd -e service_name=mrt-audit >> $OUTFILE

ansible-playbook report-deployed-webapp.yaml -l $uc3_mrt_inventory_stg -e service_name=mrt-inventory >> $OUTFILE
ansible-playbook report-deployed-webapp.yaml -l $uc3_mrt_inventory_prd -e service_name=mrt-inventory >> $OUTFILE

ansible-playbook report-deployed-webapp.yaml -l $uc3_mrt_ingest_stg -e service_name=mrt-ingest >> $OUTFILE
ansible-playbook report-deployed-webapp.yaml -l $uc3_mrt_ingest_prd -e service_name=mrt-ingest >> $OUTFILE

ansible-playbook report-deployed-webapp.yaml -l $uc3_mrt_replic_stg -e service_name=mrt-replic >> $OUTFILE
ansible-playbook report-deployed-webapp.yaml -l $uc3_mrt_replic_prd -e service_name=mrt-replic >> $OUTFILE

ansible-playbook report-deployed-webapp.yaml -l $uc3_mrt_store_stg -e service_name=mrt-store >> $OUTFILE
ansible-playbook report-deployed-webapp.yaml -l $uc3_mrt_store_prd -e service_name=mrt-store >> $OUTFILE
```


----------------------------------------------------




### TODO

DONE - report which AZ is processing at each control loop iteration
DONE - when running in noop, report targetgroup configs and systemctl status.
- when restarting a service not in an ALB, validate the service is running properly (how?)
- when restarting a ALB managed service, make sure there are at least two targets in the targetgroups before running in any AZ.
DONE - add optional deploy task for Tomcat Webapps.
DONE - add playbook to report configured vs. deployed build tag for Tomcat webapps.
- validate the service_name is for a Tomcat webapp when running in deploy mode.
- preview configured rev vs. deployed rev when running in deploy mode.
- when running in deploy mode if configure rev and deployed rev are the same, then skip.
