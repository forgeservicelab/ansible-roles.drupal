Drupal
======

Installs drupal on a Highly Available environment by means of a shared NFS mount to hold the drupal directory tree.

Requirements
------------

This role is a client to a pre-existing NFS share and it does not set up a NFS server. The exported folder is expected to be `/data/exports` on the NFS server.

As per drupal's system requirements, a MySQL server with root access allowed from the provisioned instance is needed.

Configured drupal webservers handle SSL negotiation, the SSL certificate and key files are expected under the `files` directory. Both files should share the name up until the extension.

Role Variables
--------------

- `NFS_VIRTUAL_IP` - The IP of the NFS server. Declared but undefined, it is expected to be defined on `vars/main.yml` or overriden on the global scope.
- `MySQL_VIRTUAL_IP` - The IP of the MySQL server. Declared but undefined, it is expected to be defined on `vars/main.yml` or overriden on the global scope.
- `ROUTER_PUBLIC_IP` - The IP of the public interface of the target network's router. Undefined, required only if the target environment is deployed on OpenStack.
- `certificate_prefix` - The name of both SSL certificate files excluding `.crt` and `.key` respectively. Note that the module expects the certificate file to have a .crt extension, if your file has a .pem extension, please rename it. Declared but undefined, it is expected to be defined on `vars/main.yml` or overriden on the global scope.
- `IS_TARGET_OPENSTACK` - Boolean defining whether the target environment is deployed on OpenStack. Expected to be declared on the global scope.
- `certificate_path` - Path to the SSL certificates directory, relative to Apache's directory, defaults to `tls`.
- `modules` - A list of drupal modules to install in addition to core modules. See `vars/main.yml` for more information.
- `drupal_version` - Version of drupal to install, defaults to 7.26.
- `drupal_database` - The name of the database on the MySQL server for drupal to use. Declared but undefined, it is expected to be defined on `vars/main.yml` or overriden on the global scope.
- `drupal_username` - MySQL user for drupal, defaults to `drupalcore`
- `drupal_passwd` - MySQL password for the drupal user, defaults to `drupalpass`
- `cas_version` - Version of additional CAS module, defaults to 7.x-1.3-rc1
- `cas_attributes_version` - Version of additional CAS Attributes module, defaults to 7.x-1.0-rc3
- `ldap_version` - Version of additional LDAP module, defaults to 7.x-2.0-beta8
- `ctools_version` - Version of additional Chaos Tools module, defaults to 7.x-1.3
- `entity_api_version` - Version of additional Entity API module, defaults to 7.x-1.3
- `token_version` - Version of additional Token module, defaults to 7.x-1.5
- `role_delegation_version` - Version of additional Role Delegation module, defaults to 7.x-1.1
- `primary` - Flag that indicates the target host in which to run tasks that only need to be executed in one node. This is expected to be an inventory variable within the host group.

Dependencies
------------

The [common role](https://git.forgeservicelab.fi/ansible-roles/common) is defined as a dependency, make sure it is present in your playbook.

Variables read from other roles:

- `IS_TARGET_OPENSTACK` - Expected to be declared on the global scope.

Author Information
------------------

[Forge Servicelab Team](http://forgeservicelab.fi)
