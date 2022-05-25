---
layout: post
title: "WordPress Setup Checklist"
title_short: "WordPress Setup Checklist"
excerpt: >
  Checklist for setting up WordPress from scratch on a Debian or Ubuntu virtual
  machine.
---

{{ page.excerpt }}

1.  Install OS, or choose the appropriately sized EC2 or GCE system.
1.  Update current OS packages:

        sudo apt-get update && sudo apt-get upgrade && sudo apt-get autoclean && sudo apt-get autoremove

1.  Install the following packages:
    1.  apache2
    1.  mysql-server
        1.  Record the root password to an encrypted file.
    1.  php5 php5-mysql
1.  Apache Setup
    1.  sudo a2enmod rewrite
1.  ```cd /var/www```
1.  ```sudo wget https://wordpress.org/latest.tar.gz```
1.  ```sudo tar -xzvf latest.tar.gz```
1.  ```sudo rm -rf html```
1.  ```sudo mv wordpress html```
1.  ```sudo chown -R www-data: html/```
1.  ```sudo rm latest.tar.gz```
1.  Setup a MySQL user with access to only the WordPress database.
    1.  Change the default name of the wordpress database during setup.
    1.  Use a MySQL username that is distinct from the administrator's login.
    1.  Use a MySQL password that is distinct from the administrator's password.
1.  Open a browser to http://YUR.SER.VER.ADD <- Not literally, put in your new
    system's IP address.