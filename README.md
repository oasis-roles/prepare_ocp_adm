[![Build Status](https://travis-ci.com/oasis-roles/prepare_ocp_adm.svg?branch=master)](https://travis-ci.com/oasis-roles/prepare_ocp_adm)

prepare\_ocp\_adm
===========

This follows the [documentation](https://docs.openshift.com/container-platform/3.11/install/host_preparation.html)
steps for deploying the [OpenShift Container Platform](https://www.openshift.com).
This role applies specifically to a deploy host that will drive the deployment
and yet not become part of the cluster

Requirements
------------

Ansible 2.4 or higher

Red Hat Enterprise Linux 7 or equivalent

Valid Red Hat Subscriptions

Role Variables
--------------

Currently the following variables are supported:

### General

* `prepare_ocp_adm_become` - Default: true. If this role needs administrator
  privileges, then use the Ansible become functionality (based off sudo).
* `prepare_ocp_adm_become_user` - Default: root. If the role uses the become
  functionality for privilege escalation, then this is the name of the target
  user to change to.

Dependencies
------------

The adm system needs to already be configured with [subscription-manager](https://github.com/oasis-roles/rhsm)
to include the necessary repositories as described in the documentation. Additionally the local
Ansible inventory and group\_vars folders need to already exist on the local system so that they
can be uploaded to the remote target. This role will install the "openshift-ansible" package, so
whichever repositories provide the particular version of that package to correspond with the version
of OCP you are trying to deploy.

Additionally, you need to have a local copy of the SSH private key file that will be used to
log in to the remote hosts, if these are not explicitly defined elsewhere in group\_vars and
the inventory file. Obviously, if other methods of authentication are in use, then those are
outside of the scope of this playbook.

Example Playbook
----------------

```yaml
- hosts: prepare_ocp_adm-servers
  roles:
    - role: oasis_roles.prepare_ocp_adm
```

License
-------

GPLv3

Author Information
------------------

Greg Hellings <greg.hellings@gmail.com>
