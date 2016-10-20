# Description

This vagrant setup helped to develop against a complex LDAP setup. For now it is an initial draft, wait of the things to come.

# Usage

To fire up both LDAP servers, issue:

`vagrant up`

After firing up both VMs you can query the LDAP servers within each VM using:

`ldapsearch -x -b dc=eu,dc=datameer,dc=example -D cn=admin,dc=eu,dc=datameer,dc=example -w 1234`

or

`ldapsearch -x -b dc=apac,dc=datameer,dc=example -D cn=admin,dc=apac,dc=datameer,dc=example -w 1234`


For convenience I forwarded both LDAP servers to the host machine:


`ldapsearch -x -H ldap://localhost:3891 -b dc=eu,dc=datameer,dc=example -D cn=admin,dc=eu,dc=datameer,dc=example -w 1234`

or

`ldapsearch -x -H ldap://localhost:3892 -b dc=apac,dc=datameer,dc=example -D cn=admin,dc=apac,dc=datameer,dc=example -w 1234`

# Future Plans

* Populate servers with data
* Make admin name and password parameterized
* Improve documentation

