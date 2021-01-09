
This is the consolez package for z/VM and zLinux.  

It is a Web-UI to give team members access to just the z/VM consoles and commands they need.
The line commands *must* go in /usr/local/sbin/. The Web CGIs default to /srv/www/ldap/ but they 
don't have to go there. Here is a sample Apache configuration (on RHEL 8) setting up LDAP 
authentication for that directory: 
 
The real README is now README.md in the root consolez directory.
