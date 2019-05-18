[![Build Status](https://travis-ci.com/oasis-roles/prepare_ocp_adm.svg?branch=master)](https://travis-ci.com/oasis-roles/prepare_ocp_adm)

prepare\_ocp\_adm
===========

This follows the [documentation](https://docs.openshift.com/container-platform/3.11/install/host_preparation.html)
steps for deploying the [OpenShift Container Platform](https://www.openshift.com).
This role applies specifically to a deploy host that will drive the deployment
and yet not become part of the cluster

After invoking this role, the adm host will be ready for invoking the openshift-ansible
playbooks against the other hosts in the cluster. An invocation such as the following
should do the trick

```bash
cd /usr/share/ansible/openshift-ansible/playbooks
# export ANSIBLE_CONFIG=/etc/ansible/ansible.cfg
ansible-playbook prerequisites.yml
ansible-playbook deploy_cluster
```

Substitute in the location of `{{ prepare_ocp_adm_destination }}/ansible.cfg` in the
above execution, and all the other components of this role should automatically be
configured. If you use the default value for desination than you can skip the export
step, as /etc/ansible/ansible.cfg is the default location Ansible will search in
for a config file. If you change the location of destination, then you will need
to export the environment variable.

Requirements
------------

Ansible 2.4 or higher

Red Hat Enterprise Linux 7 or equivalent

Valid Red Hat Subscriptions

Role Variables
--------------

Currently the following variables are supported:

### General

* `prepare_ocp_adm_inventory_file` - Default: current inventory file being used
  to run this playbook. This should point to the inventory file for the cluster
  that will be uploaded to the adm host after it is configured.
* `prepare_ocp_adm_destination` - Default: /etc/ansible. Path to the folder where
  inventory file, group vars, ansible.cfg, ssh key, etc will be uploaded.
* `prepare_ocp_adm_target_user` - Default: root. The user who will be running the
  eventual install of OpenShift. This user needs to have write privileges to the
  `prepare_ocp_adm_destination` locale.
* `prepare_ocp_adm_ssh_private_key_file` - Default: ansible\_ssh\_private\_key\_file.
  This is the private SSH key that the adm role will use to connect to the rest of
  the cluster during OpenShift installation.
* `prepare_ocp_adm_ansible_cfg`. A dictionary of ansible.cfg key:value pairs that
  will be set in the ansible.cfg file added to the destination. While it is not
  imperative to invoke Ansible with this file, doing so will automatically point
  your playbook to the uploaded inventory file, private key file, and logs
  directory on the adm host.
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
