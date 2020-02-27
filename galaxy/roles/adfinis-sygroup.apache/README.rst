===========
ROLE APACHE
===========

.. image:: https://img.shields.io/github/license/adfinis-sygroup/ansible-role-apache.svg?style=flat-square
  :target: https://github.com/adfinis-sygroup/ansible-role-apache/blob/master/LICENSE

.. image:: https://img.shields.io/travis/adfinis-sygroup/ansible-role-apache.svg?style=flat-square
  :target: https://github.com/adfinis-sygroup/ansible-role-apache

.. image:: https://img.shields.io/badge/galaxy-adfinis--sygroup.apache-660198.svg?style=flat-square
  :target: https://galaxy.ansible.com/adfinis-sygroup/apache

A brief description of the role goes here.


Requirements
=============

Any pre-requisites that may not be covered by Ansible itself or the role
should be mentioned here. For instance, if the role uses the EC2 module, it
may be a good idea to mention in this section that the boto package is required.


Role Variables
===============

Listen IP and port:

* ``apache_listen_ip``: The IP to listen on
* ``apache_listen_port``: Port to listen on for non-SSL vhosts
* ``apache_listen_port_ssl``: Listen port vor SSL vhosts

Vhost configuration files:

* ``apache_vhosts_filename``: Name of the vhost configuration file created on
  the host
* ``apache_vhosts_template``: Name of the vhost configuration template in the
  ansible ``templates`` directory

Vhost default options (applies to all vhosts unless overriden):

* ``apache_allow_override``: Default ``allow_override`` setting to allow overrides
  in ``.htaccess`` files
* ``apache_options``: Default apache options that can be overridden in a particular
  vhost configuration

``apache_vhosts`` defines all Apache virtual hosts (non-ssl vhost). Each vhost contains
the required variable ``servername`` to uniquely identify the vhost. All other
variables are optional and defined as follows:

* ``serveralias``: Alias or alternative name for the host
* ``documentroot``: Main document tree
* ``redirect_ssl``: Redirect to SSL enabled vhost
* ``redirect_ssl_vhost``: Redirect to this enabled vhost
* ``options``: Vhost apache options that override the default options of the
  global variable ``apache_options``
* ``serveradmin``: Email address of server admin

SSL Variables
-------------

``apache_vhosts_ssl`` defines all SSL-enabled Apache virtual hosts, supporting
the same variables as ``apache_vhosts`` (except for ``redirect_ssl``).
Additionally, SSL key and certificate need to be specified.

SSL variables specific to a vhost in ``apache_vhosts_ssl``:

* ``certificate_file``: SSL certificate and chain file
* ``certificate_key_file``: SSL key file
* ``certificate_chain_file``: Separate chain file, deprecated since apache 2.4.8

Global SSL variables (not vhost specific):

* ``apache_ssl_protocol``: Allowed SSL protocol versions
* ``apache_ssl_cipher_suite``: Allowed SSL cipher suites

Modules
-------

The playbook creates module symlinks into the ``/etc/apache2/mods-enabled/``
directory for Debian based hosts. Modules listed in ``apache_mods_enabled`` ar
enabled by default:

* ``rewrite.load``: Rewrite module
* ``ssl.load``: SSL module

Additional Parameters
---------------------
Any configuration directive not covered by the variables above can be appended
to the ``apache_vhosts.extra_options`` and ``apache_vhosts_ssl.extra_options``
variables using a `literal block scalar
<https://docs.ansible.com/ansible/latest/reference_appendices/YAMLSyntax.html>`_
as follows:

.. code-block:: yaml

  extra_options: |
    <Location "/status">
      SetHandler server-status
      Require host example.com
    </Location>

These extra options are appended right before the closing
``<VirtualHost>`` section.

Dependencies
=============

* Role: `adfinis-sygroup.pki <https://github.com/adfinis-sygroup/ansible-role-pki>`_

Example Playbook
=================

Including an example of how to use your role (for instance, with variables
passed in as parameters) is always nice for users too:

.. code-block:: yaml

  - hosts: servers
    roles:
       - { role: adfinis-sygroup.apache }


License
========

`GPL-3.0 <https://github.com/in0rdr/ansible-role-apache/blob/master/LICENSE>`_


Author Information
===================

apache role was written by:

* Adfinis SyGroup AG | `Website <https://www.adfinis-sygroup.ch/>`_ | `Twitter <https://twitter.com/adfinissygroup>`_ | `GitHub <https://github.com/adfinis-sygroup>`_
