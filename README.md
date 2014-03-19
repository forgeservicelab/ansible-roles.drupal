Drupal
======

Installs drupal on a Highly Available environment by means of a shared NFS mount to hold the drupal directory tree.

Requirements
------------

This role is a client to a pre-existing NFS share and it does not set up a NFS server. The exported folder is expected to be `/data/exports` on the NFS server.

As per drupal's system requirements, a MySQL server with root access allowed from the provisioned instance is needed.

Configured drupal webservers handle SSL negotiation, the SSL certificate and key files are expected under the `files` directory. All files should share the name up until the extension. The expected files are:
  - `{{ certificate_prefix }}.crt` - The server SSL certificate file.
  - `{{ certificate_prefix }}.key` - The private key for the certificate.
  - `{{ certificate_prefix }}.chain.pem` - The intermediate CA chain file.

Role Variables
--------------

- `NFS_VIRTUAL_IP` - The IP of the NFS server. Declared but undefined, it is expected to be defined on `vars/main.yml` or overriden on the global scope.
- `MySQL_VIRTUAL_IP` - The IP of the MySQL server. Declared but undefined, it is expected to be defined on `vars/main.yml` or overriden on the global scope.
- `ROUTER_PUBLIC_IP` - The IP of the public interface of the target network's router. Undefined, required only if the target environment is deployed on OpenStack.
- `certificate_prefix` - The name of both SSL certificate files excluding `.crt` and `.key` respectively. Note that the module expects the certificate file to have a .crt extension, if your file has a .pem extension, please rename it. Declared but undefined, it is expected to be defined on `vars/main.yml` or overriden on the global scope.
- `IS_TARGET_OPENSTACK` - Boolean defining whether the target environment is deployed on OpenStack. Expected to be declared on the global scope.
- `certificate_path` - Path to the SSL certificates directory, relative to Apache's directory, defaults to `tls`.
- `drupal_database` - The name of the database on the MySQL server for drupal to use. Declared but undefined, it is expected to be defined on `vars/main.yml` or overriden on the global scope.
- `drupal_username` - MySQL user for drupal, defaults to `drupalcore`
- `drupal_passwd` - MySQL password for the drupal user, defaults to `drupalpass`
- `primary` - Flag that indicates the target host in which to run tasks that only need to be executed in one node. This is expected to be an inventory variable within the host group.

Dependencies
------------

The [common role](https://git.forgeservicelab.fi/ansible-roles/common) is defined as a dependency, make sure it is present in your playbook.

Variables read from other roles:

- `IS_TARGET_OPENSTACK` - Expected to be declared on the global scope.

Author Information
------------------

[Forge Servicelab Team](http://forgeservicelab.fi)
