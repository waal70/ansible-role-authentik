waal70.authentik
=========

This role will deploy and configure Authentik into a Portainer install

Requirements
------------

Authentik will be deployed through Portainer, as described in waal70.portainer. That role makes use of waal70.docker.

Backup and restore
------------------

* Backup /media folder: holds icons, flow backgrounds and uploaded files
* Optional: backup /certs, /custom-templates and /blueprints, but only if you use them
* Make sure nobody is actively using authentik. To be absolutely sure, you may stop all stack containers, except the db
* Go into the console for the database image of portainer and cd into the dbdumps folder (```cd /dbdumps```)
* Perform ```pg_dump authentik -U authentik > backupdump.sql```
* Exit the Portainer db console and move into the SSH console
* Secure the backupdump.sql in a good place

Performing the actual restore with aid of this role
---------------------------------------------------

* Install authentik through this role onto a fresh server instance.
* Let the containers come to a full start (```healthy```) so that all defaults may be initialized
* Stop all containers of the stack in Portainer, except the db. Do not stop the stack as your containers will disappear!
* In your shell, move the database dump into the volume mapping (```mv backupdump.sql /appdata/authentik/dbdumps/```)
* Open a console in Portainer to the database container. Move into the dbdumps folder (```cd /dbdumps```)
* Drop the existing database: ```dropdb authentik -U authentik```
* Create an empty database: ```createdb -T template0 authentik -U authentik```
* Perform a restore: ```psql -X authentik -U authentik < backupdump.sql```
* Start the complete stack and enjoy your restored Authentik!

Dependencies
------------

waal70.portainer

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: servers
      roles:
         - { role: username.rolename, x: 42 }

License
-------

[GPLv3](https://www.gnu.org/licenses/gpl-3.0.html#license-text)

Author Information
------------------

Unless otherwise noted, this entire repository is (c) 2025 by André (waal70). [See github profile](https://github.com/waal70)

Please contact me if you need a commercial license for any of these files
