Ansible Playbooks for OwnCloud installation
===========================================

This ansible playbook installs a complete ownCloud infrastructure. It is similar
to the one used at SWITCH, as SWITCHdrive.

Requirements
------------

For detailed instructions on what kind of servers you need, see the following
document: (SWITCHdrive Architecture)[1]

The (installation document)[2] covers the process of setting up your local
workstation with Ansible, the SSH requirements etc. and describes in detail how
this project is structured

Installation
------------

Change the `staging` and `production` files to match your setup. This includes
IP adresses and variables.

Copy the file `vars/users_example.yml` to `vars/users.yml` and add the names
and keys for the admins and the developers that will need to have access to the server.

Follow the (installation document)[2] to see what should be configured. If your setup
differs, you will need to adapt some of the playbooks.


To install a complete system, run the following command locally:

    ansible-playbook -i testing -s site.yml

where `testing` is the name of the environment, and `site.yml` the name
of the file that describes the complete system

or individual server groups like so:

    ansible-playbook -i testing -s webservers.yml


Environments
------------

We use two environments - you can choose which environment to perform work on:

  * testing
  * production

Server Types
------------

* dbservers.yml         - the Postgres Database Servers
* devservers.yml        - a Web server that is used for development and not included
                          in the load balancer. Should have a public IP address
* lbservers.yml         - the HAproxy loadbalancer
* ldapservers.yml       - the LDAP server
* monitoringservers.yml - the Zabbix Monitoring server
* nfsservers.yml        - the NFS server
* webservers.yml        - the nginx/php5-fpm Web/App server combo


Tasks
-----

There are a number of tasks you can perform on the application:

    ansible-playbook -i testing -s taskname.yml

where taskname.yml is one of the following:

* maintenance_start.yml     - put ownCloud into maintenance mode
* maintenance_stop.yml      - disable ownCloud maintenance mode
* servers_start.yml         - start the web servers
* servers_stop.yml          - stop the web servers (the load balancer will show a 503 page)
* backup_db.yml             - pull a backup of the postgres database to your local workstation



[1]: https://github.com/switch-ch/cloudservice-owncloud/blob/master/source/architecture.rst
[2]: https://github.com/switch-ch/cloudservice-owncloud/blob/master/source/installation.rst
