# Description

This vagrant setup helped to develop against a complex LDAP setup. It simulates 4 LDAP servers which all contain region specific users and groups.
The regions are: EU, US and APAC. The EU region is represented by two servers (eu1 and eu2). APAC and US have each 1 server associated to them.

# Requirements

* VirtualBox >= 5.1.6
* vagrant 1.8.7
* ansible >= 2.0.0.2

# LDAP server configs

* 4 LDAP server
  * locahost:3891
  * search base: dc=eu,dc=company,dc=example
  * login: admin,dc=eu,dc=company,dc=example
  * password: 1234
* locahost:3892
* search base: dc=eu,dc=company,dc=example
* login: admin,dc=eu,dc=company,dc=example
* password: 1234
* locahost:3893
* search base: dc=eu,dc=company,dc=example
* login: admin,dc=eu,dc=company,dc=example
* password: 1234
* locahost:3894
* search base: dc=eu,dc=company,dc=example
* login: admin,dc=eu,dc=company,dc=example
* password: 1234


# Usage

To fire up both LDAP servers, issue:

`vagrant up`

In case you get an error stating that Vagrant is not able to download bento/ubuntu-16.04, you have to remove the curl bundled with vagrant:

`sudo rm -rf /opt/vagrant/embedded/bin/curl`

Depending on the vagrant-box version you are downloading, you might have to upgrade to VirtualBox 5.1.6. There was a problem with version box v2.3.0 failing with VirtualBox below version 5.1.6

## Query the LDAP servers as admin

You can query the EU server like:

`ldapsearch -x -H ldap://localhost:3891 -b dc=eu,dc=company,dc=example -D cn=admin,dc=eu,dc=company,dc=example -w 1234`
`ldapsearch -x -H ldap://localhost:3892 -b dc=eu,dc=company,dc=example -D cn=admin,dc=eu,dc=company,dc=example -w 1234`

Note, the only difference between the users on these servers are the email adresses. Every username and group that is present on eu1 is also present in eu2.

APAC and US are structured accordingly:

`ldapsearch -x -H ldap://localhost:3893 -b dc=apac,dc=company,dc=example -D cn=admin,dc=apac,dc=company,dc=example -w 1234`
`ldapsearch -x -H ldap://localhost:3894 -b dc=us,dc=company,dc=example -D cn=admin,dc=us,dc=company,dc=example -w 1234`

## Login as a regular user

To login as one of the generated users you invoke:

`ldapsearch -x -b dc=us,dc=company,dc=example -H ldap://localhost:3894 -D cn=A1,ou=user,dc=us,dc=company,dc=example -w A1 cn=A1`

# Future Plans

* Make admin name and password parameterized

