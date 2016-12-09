# Description

This vagrant setup helped to develop against a complex LDAP setup. It simulates 4 LDAP servers which all contain region specific users and groups.
The regions are: EU, US and APAC. The EU region is represented by two servers (eu1 and eu2). APAC and US have each 1 server associated to them.

# Requirements

* VirtualBox >= 5.1.6
* vagrant 1.8.7
* ansible >= 2.0.0.2

# LDAP server configs

* 4 LDAP server
  * user definition: objectClass=Inetorgperson
  * user name attribute: cn
* locahost:3891
  * search base: dc=eu,dc=company,dc=example
  * login: admin,dc=eu,dc=company,dc=example
  * password: 1234
* locahost:3892
  * search base: dc=eu,dc=company,dc=example
  * login: admin,dc=eu,dc=company,dc=example
  * password: 1234
* locahost:3893
  * search base: dc=apac,dc=company,dc=example
  * login: admin,dc=apac,dc=company,dc=example
  * password: 1234
* locahost:3894
  * search base: dc=us,dc=company,dc=example
  * login: admin,dc=us,dc=company,dc=example
  * password: 1234


# Usage

After you have installed all the required software you can start both LDAP servers by running:

`vagrant up`

In case you get an error stating that Vagrant is not able to download bento/ubuntu-16.04, you have to remove the curl bundled with vagrant:

`sudo rm -rf /opt/vagrant/embedded/bin/curl`

Depending on the vagrant-box version you are downloading, you might have to upgrade to VirtualBox 5.1.6. There was a problem with version box v2.3.0 failing with VirtualBox below version 5.1.6

## Using the LDAP servers as admin

You can query the EU server as an admin with:

`ldapsearch -x -H ldap://localhost:3891 -b dc=eu,dc=company,dc=example -D cn=admin,dc=eu,dc=company,dc=example -w 1234`

`ldapsearch -x -H ldap://localhost:3892 -b dc=eu,dc=company,dc=example -D cn=admin,dc=eu,dc=company,dc=example -w 1234`

APAC and US are structured accordingly:

`ldapsearch -x -H ldap://localhost:3893 -b dc=apac,dc=company,dc=example -D cn=admin,dc=apac,dc=company,dc=example -w 1234`

`ldapsearch -x -H ldap://localhost:3894 -b dc=us,dc=company,dc=example -D cn=admin,dc=us,dc=company,dc=example -w 1234`

## Login as a regular user

The users on all 4 LDAP servers are prefixed with there region so that no two users have the same name on two different LDAP servers.

The username of each user ends with a number. The characters preceeding the number denote the group this user is a member of. E.g. User US-ABA1 is member of the group US-ABA.
The group US-ABA however, is itself member of US-AB, which is by itself member of US-A. I hope you get the idea. The user and group names directly give you the information about membership. 

To login as one of the generated users you invoke:

`ldapsearch -x -b dc=us,dc=company,dc=example -H ldap://localhost:3894 -D cn=US-A1,ou=user,dc=us,dc=company,dc=example -w A1 cn=A1`

Each users password is equal to its username without the region prefix.

# Future Plans

* Make admin name and password parameterized

