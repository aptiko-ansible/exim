====
Exim
====

Description
===========

This is an Ansible role for installing exim on a Debianserver.  It
also sets up the root email alias in ``/etc/aliases``. It can be set
either to only send emails (useful for enabling a machine to send
administrative email, such as cron error messages, to the admins), or
to be a mail server that receives and sends user email.

Examples
========

Configuration example, send only, with a smart host::

   exim_configtype: satellite
   exim_mailname: mydomain.com
   admins: myemail@somewhere.com
   exim_smarthost: mail.itia.ntua.gr::587
   exim_smarthost_rdns: '*.ntua.gr'
   exim_smarthost_username: myuser
   exim_smarthost_password: topsecret

Configuration example, send only, directly::

   exim_configtype: internet
   exim_mailname: mydomain.com
   admins: myemail@somewhere.com

Configuration example, receiving and sending mail server::

   exim_configtype: internet
   exim_mailname: mydomain.com
   admins: myemail@somewhere.com
   exim_receive_mail: yes
   exim_be_smarthost: yes
   exim_key: |
     -----BEGIN RSA PRIVATE KEY-----
     ...
     -----END RSA PRIVATE KEY-----
   exim_crt: |
     -----BEGIN CERTIFICATE-----
     ...
     -----END CERTIFICATE-----
   exim_chain_certificates: |
     -----BEGIN CERTIFICATE-----
     ...
     -----END CERTIFICATE-----
     -----BEGIN CERTIFICATE-----
     ...
     -----END CERTIFICATE-----


Reference
=========

The following variables are used:

- ``exim_configtype``: Either "internet" or "satellite".
- ``exim_mailname``: The mailname.
- ``exim_other_hostnames``: A colon-separated list of domain names.
  This is the list of domains for which this machine should consider
  itself the final destination. If unspecified, ``exim_mailname`` is
  used.
- ``admins``: A comma-separated list of emails, for which "root" will
  be an alias.
- ``exim_smarthost``: Required for satellite. The smarthost, optionally
  followed by double colon and port.
- ``exim_smarthost_rdns``, ``exim_smarthost_username``,
  ``exim_smarthost_password``: Necessary when the
  smarthost requires authentication.  The content of these variables
  is set in ``/etc/exim4/passwd.client``.
- ``exim_receive_mail``: True if the system should receive email. In
  that case, it listens on all interfaces, and the firewall also
  allows port 25. Default false.
- ``exim_be_smarthost``: True if the system should be a smarthost. In
  this case it listens on port 587, where it authenticates users and
  relays their emails. If false (the default), it does not listen on
  port 587.
- ``exim_key``, ``exim_crt``, ``exim_chain_certificates``: Encryption
  keys. Required when ``exim_be_smarthost`` is true.

Meta
====

Written by Antonis Christofides

| Copyright (C) 2014 National Technical University of Athens
| Copyright (C) 2014 TEI of Epirus
| Copyright (C) 2014-2015 Antonis Christofides

This program is free software: you can redistribute it and/or modify
it under the terms of the GNU General Public License as published by
the Free Software Foundation, either version 3 of the License, or
(at your option) any later version.

This program is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
GNU General Public License for more details.

You should have received a copy of the GNU General Public License
along with this program.  If not, see http://www.gnu.org/licenses/.
