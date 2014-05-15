Ansible Playbooks for OwnCloud installation
===========================================

This ansible playbook installs a complete ownCloud infrastructure. It is similar
to the one used at SWITCH, as SWITCHdrive.

Requirements
------------

For detailed instructions on what kind of servers you need, see the following
document: [SWITCHdrive Architecture][1]

The [installation document][2] covers the process of setting up your local
workstation with Ansible, the SSH requirements etc. and describes in detail how
this project is structured

Installation
------------

Change the `staging` and `production` files to match your setup. This includes
IP addresses and variables.

Copy the file `vars/users_example.yml` to `vars/users.yml` and add the names
and keys for the admins and the developers that will need to have access to the server.

Follow the [installation document][2] to see what should be configured. If your setup
differs, you will need to adapt some of the playbooks.


To install a complete system, run the following command locally:

    ansible-playbook -i testing -s site.yml

where `testing` is the name of the environment, and `site.yml` the name
of the file that describes the complete system

or individual server groups like so:

    ansible-playbook -i testing -s webservers.yml

OwnCloud is a bit stupid about its config files. In order to get the initial installation to
work, ssh to one of the web servers, then delete `var/www/config/config.php` and open your
owncloud installation from a web browser. OwnCloud will now install and setup a working `config.php`.

Open it and copy the values for `instanceid`, `passwordsalt` and `trusted_domains` to the config
files in `roles/owncloud/templates/TLD/config.php.j2` then re-run the playbook that sets up the
webservers:

    ansible-playbook -i testing -s webservers.yml

You will need to do a similar dance when you upgrade ownCloud, as the update process changes the
version number in the owncloud config file.

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

    ansible-playbook -i testing -s scripts/taskname.yml

where taskname.yml is one of the following:

* maintenance_start.yml     - put ownCloud into maintenance mode
* maintenance_stop.yml      - disable ownCloud maintenance mode
* servers_start.yml         - start the web servers
* servers_stop.yml          - stop the web servers (the load balancer will show a 503 page)
* backup_db.yml             - pull a backup of the postgres database to your local workstation

Contributing
------------

1. Fork it ( https://github.com/switch-ch/owncloud-ansible/fork )
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Add some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create a new Pull Request

[1]: https://github.com/switch-ch/cloudservice-owncloud/blob/master/source/architecture.rst
[2]: https://github.com/switch-ch/cloudservice-owncloud/blob/master/source/installation.rst
