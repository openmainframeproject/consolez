
This is the consolez package for z/VM and zLinux.  

It is a Web-UI to give team members access to just the z/VM consoles and commands they need.
The line commands *must* go in /usr/local/sbin/. The Web CGIs default to /srv/www/ldap/ but they 
don't have to go there. Here is a sample Apache configuration (on RHEL 8) setting up LDAP 
authentication for that directory: 
```
# cat /etc/httpd/conf/httpd.conf
...
# ldap/ directory is for LDAP-protected scripts
ScriptAlias /ldap/ /srv/www/ldap/
<Directory /srv/www/ldap/>
    AllowOverride None
    Options +ExecCGI -Includes
    AuthType Basic
    AuthName "Consolez - Enter your LDAP credentials"
    AuthBasicProvider ldap
    AuthLDAPURL ldap://<your.orgs.ldap.server:389/ou=people,ou=linux_systems,dc=example,dc=com?uid
    Require ldap-filter objectClass=posixAccount
</Directory>
...
```
If you don't have LDAP authentication, you can set up password files. You could also just copy the scripts to cgi-bin/ and leave them wide open.  (Not recommended, but your choice :))

You will need:
- A directory with a lot of free space
- A Linux user that has SSH key-based (passwordless) authentication set up
- A Linux server with elevated privileges, Apache running and sudo entries to enable these commands:
  - /bin/rm
  - /bin/touch
  - /sbin/chccwdev
  - /sbin/vmcp
  - /usr/bin/chmod
  - /usr/bin/chown
  - /usr/sbin/vmur

Then copy the file /etc/consolez.conf.sample to /etc/consolez.conf and configure it:
Here is what's currently in the sample config file:
```
#
# Configuration file for consolez - /etc/consolez.conf
#
# Variable 'consolesUser' must be set to a user that can SSH without passwords
# Variable 'consolezDir' can be set but if not will default to /srv/consolez
#
consolezDir = "/data/consolez"             # directory where console data is stored
consolezUser="zadmin"                      # user with passwordless SSH consolez will run as
#
# Remaining values define your organizations environments, Linux 'engineering servers' and z/VM systems
#
# For example, to define two environments each with two z/VM LPARs:
# environment ENV1
# hostname1 zvm1
# hostname2 zvm2
#
# environment ENV2
# hostname3 zvm3
# hostname4 zvm4
#
```
The first release was on December 25th, 2020. 
Let's call it version 0.90 so there will be ten tries to get it to version 1.0  :))

If you have questions, comments or bug reports, you can email me at mike99mac at gmail.com  -Mike MacIsaac

