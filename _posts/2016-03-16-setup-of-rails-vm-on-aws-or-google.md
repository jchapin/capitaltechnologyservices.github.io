---
layout: post
title: 'Setup of Standard Ruby / Rails App Virtual Machine on EC2 or Google Cloud'
title_short: 'Setup of Standard Ruby / Rails App VM'
excerpt: "A set of instructions compiled for a client setting up their own virtual machine for hosting a Rails application on Amazon EC2 or Google Cloud."
---

Typically we prefer working with a Platform as a Service such as Heroku or AWS
Beanstalk for hosting applications. But a client with a unique set of
circumstances needed an EC2 instance for an application deployment. Here are the
directions that we provided to get them up and running.

## Setting up the EC2 Instance

1.  If you haven't already setup an account on Amazon Web Services go to
    <https://aws.amazon.com>, register and connect a credit card to the account.
1.  Once registered, make your way to the, "AWS Management Console" and choose
    "EC2 - Virtual Servers in the Cloud" which is in the upper left hand corner
    of the screen.
1.  In the middle of the screen underneath, "Create Instance" click the blue
    button that says, "Launch Instance"
1.  From the list of options on the, "Quick Start" tab choose,
    "Ubuntu Server 14.04 LTS (HVM), SSD Volume Type" by clicking on the blue
    button on the right hand side of the screen.
1.  For the application we're working with, the minimum memory recommended is
    4GB, and 8GB is highly recommended. I would choose a, "t2.large" system.
    1.  Pricing information can be found here:
        <https://aws.amazon.com/ec2/pricing/>
    1.  I would recommend converting the instance to a, "reserved" instance and
        prepaying for the service, it will reduce the bill for the services
        considerably (40-50%).
1.  Instead of clicking, "Review and Launch", go to,
    "Next: Configure Instance Details" to tweak specific details of the
    configuration.
1.  On the screen, "Step 3: Configure Instance Details" the only thing you
    might want to consider checking are, "Enable CloudWatch detailed monitoring"
    so that you can get detailed information about the application's performance
    and use the information to make any changes to the application's environment
    or usage based on its resource usage after launch. This can be turned off
    later.
1.  Click, "Next: Add Storage" in the lower right hand corner.
1.  The configuration will default to 8GB of disk space. Since we had originally
    planned on storing image assets on Amazon S3, and the database isn't very
    large, you don't need a very large storage volume. But I would recommend
    40 to 50GiB. The form input for altering this is dead center in the middle
    of the screen.
1.  Click, "Next: Tag Instance" and in the, "Value" field on the right hand side
    enter a name for the machine.
1.  Click, "Next: Configure Security Group".
1.  If you don't already have a Security Group, then create a new group with at
    least the following three rules:
    1.  Allow SSH from anywhere for now. You will eventually want to restrict
        this to the IP address(es) of the office. But we'll need access from
        Pennsylvania and Tennessee for a little while.
    1.  Click, "Add Rule" and allow HTTP from anywhere.
        1.  We are going to immediately forward traffic off of HTTP to HTTPS
            once a user arrives.
    1.  Click, "Add Rule" and allow HTTPS from anywhere.
1.  Click, "Review and Launch"
1.  Review the details of the instance, confirm they're all correct and click,
    "Launch" in the lower right hand corner of the screen.
1.  A modal dialogue window will pop up and ask you what key pair you want to
    setup for accessing the system.
    1.  Create a new key pair.
    1.  The key will be presented for download.
    1.  Never openly share this key pair, but keep a backup in a safe place at
        your office.
1.  Click "Launch Instances"
1.  Review the documentation on the "Launch Status" page. We will summarize how
    to connect to the machine in the next steps as well.
1.  Click "View Instances" and go to the listing of all of your EC2 instances.
1.  Click the instance that you just setup and look for the public IP address
    in the lower right hand side of the screen when the information on the
    instance loads.

## SSH Connectivity

1.  You're going to want to setup an SSH configuration file if you don't already
    have one on your system. If you're on an OS X or a Linux system, use a text
    editor to open ```~/.ssh/config```. It may not exist yet. In fact, the .ssh
    directory may not exist yet, and if that's the case you can run
    ```mkdir ~/.ssh``` and the directory will be created for you.
1.  Copy the key file from Amazon into the ```~/.ssh``` directory.
1.  In the ```~/.ssh/config``` file add the following information:

        # Description of the Machine
        Host           shortname
        Hostname       IP ADDRESS FROM AMAZON
        Port           22
        IdentitiesOnly yes
        IdentityFile   ~/.ssh/yourkey.pem
        User           ubuntu

1.  Test logging into the system by running ```ssh shortname``` that's a quick
    way to log into the system. There will likely be a warning saying that the
    authenticity of the host cannot be verified. It will present a fingerprint
    and ask you if you would like to continue connecting. Type, "yes" and
    continue connecting.
    1.  You shouldn't get a warning like this in the future when connecting from
        the same machine to this new instance unless the "known hosts" file has
        been wiped out. So be mindful of receiving warnings like this in the
        future.

## Initial Upgrade of the Environment

Once you're logged in via SSH, update the system and install any upgrades
that have been released:

1.  ```sudo apt-get update```
1.  ```sudo apt-get dist-upgrade```

## Some Help

If 

## NGINX and Passenger Installation

1.  The installation directions for Passenger+NGINX are currently located here:
    <https://www.phusionpassenger.com/library/install/nginx/install/oss/trusty/>
    1.  ```sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 561F9B9CAC40B2F7```
    1.  ```sudo apt-get install -y apt-transport-https ca-certificates```
    1.  ```sudo sh -c 'echo deb https://oss-binaries.phusionpassenger.com/apt/passenger xenial main > /etc/apt/sources.list.d/passenger.list'```

        Note that if you're installing on Ubuntu 14.04 "xenial" in the above
        line will change to "trusty". Similarly, when the next LTS release of
        Ubuntu comes out, that particular part of the line will change.

    1.  ```sudo apt-get update```
    1.  ```sudo apt-get install -y nginx-extras passenger```
1.  Once NGINX and all the requirements are installed, use a web browser to
    visit http://IP.ADDRESS.FROM.AMAZON (Substitute the actual IP address
    from Amazon)
1.  Confirm that the web server is up and running.
1.  Uncomment the following lines in ```/etc/nginx/nginx.conf```:

        # include /etc/nginx/passenger.conf;

    This above configuration file contains two lines that used to be in the main
    nginx.conf file.

        passenger_root /usr/lib/ruby/vendor_ruby/phusion_passenger/locations.ini;
        passenger_ruby /usr/bin/passenger_free_ruby;

1.  Run ```sudo service nginx restart```
1.  Run ```sudo /usr/bin/passenger-config validate-install```

## Setup Encryption via Let's Encrypt

You're most likely going to want to use a traditional encryption vendor for your
SSL Certificate. However, for the purposes of testing the NGINX SSL
configuration we will setup a certificate from Let's Encrypt (letsencrypt.org).

1.  Digital Ocean has a comprehensive set of directions here:
    <https://www.digitalocean.com/community/tutorials/how-to-secure-nginx-with-let-s-encrypt-on-ubuntu-14-04>
    1.  ```sudo apt-get -y install git bc```
    1.  ```sudo git clone https://github.com/letsencrypt/letsencrypt /opt/letsencrypt```
    1.  Stop NGINX: ```sudo service nginx stop```
    1.  ```cd /opt/letsencrypt```
    1.  ```./letsencrypt-auto certonly --standalone```
        NOTE: DNS will already have to be pointed to the hostname(s) that you
        provide when walking through the ncurses dialogue windows.

## SSL Configuration for NGINX

Next you're going to want to configure NGINX to:

1.  Forward users to an HTTPS connection automatically.
1.  Offer the most secure connectivity to the server while sacrificing as little
    compatibility with older browsers as possible.

Here are the steps:

1.  Generate a stronger certificate for Diffie-Hellman key exchange:

        cd /etc/ssl/certs
        sudo openssl dhparam -out dhparam.pem 4096

1.  Go to lunch, or otherwise take a break, generating that certificate is going
    to take a while.
1.  Scrap the default NGINX config and use the configuration below, substituting
    your certificate and key files. It uses the cipher suggestions
    from <https://raymii.org/s/tutorials/Strong_SSL_Security_On_nginx.html> that
    come from the Mozilla Foundation. That will allow browsers that are at least
    more recent than IE6 to negotiate a connection securely.

~~~
server {
  listen      80;
  server_name secure.cts-llc.net;
  return      301 https://$server_name$request_uri;
}
server {
  listen                    443;
  gzip                      on;
  gzip_proxied              any;
  gzip_types                text/css text/plain text/xml application/xml application/javascript application/x-javascript text/javascript application/json text/x-json;
  gzip_vary                 on;
  gzip_disable              "MSIE [1-6]\.";
  ssl                       on;
  ssl_certificate           /etc/letsencrypt/live/secure.cts-llc.net/fullchain.pem;
  ssl_certificate_key       /etc/letsencrypt/live/secure.cts-llc.net/privkey.pem;
  ssl_protocols             TLSv1 TLSv1.1 TLSv1.2;
  ssl_prefer_server_ciphers on;
  ssl_ciphers               "EECDH+AESGCM:EDH+AESGCM:ECDHE-RSA-AES128-GCM-SHA256:AES256+EECDH:DHE-RSA-AES128-GCM-SHA256:AES256+EDH:ECDHE-RSA-AES256-GCM-SHA384:DHE-RSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-SHA384:ECDHE-RSA-AES128-SHA256:ECDHE-RSA-AES256-SHA:ECDHE-RSA-AES128-SHA:DHE-RSA-AES256-SHA256:DHE-RSA-AES128-SHA256:DHE-RSA-AES256-SHA:DHE-RSA-AES128-SHA:ECDHE-RSA-DES-CBC3-SHA:EDH-RSA-DES-CBC3-SHA:AES256-GCM-SHA384:AES128-GCM-SHA256:AES256-SHA256:AES128-SHA256:AES256-SHA:AES128-SHA:DES-CBC3-SHA:HIGH:!aNULL:!eNULL:!EXPORT:!DES:!MD5:!PSK:!RC4";
  ssl_dhparam               /etc/ssl/certs/dhparam.pem;
  ssl_session_cache         shared:SSL:10m;
  server_name               secure.cts-llc.net;
  access_log                off;
}
~~~

This will at least get you up to an "A" rating at the moment on Qualys SSL Labs
test <https://www.ssllabs.com/ssltest/> and protect from the recent well
documented exploits. You can push the grade higher if you are willing to ditch
compatibility with some older browsers. Look at the results from the test and
make your own determination.

## Passenger Configuration

```
gem install bundler
sudo apt-get install qt5-default libqt5webkit5-dev gstreamer1.0-plugins-base gstreamer1.0-tools gstreamer1.0-x
sudo apt-get install libgmp3-dev
sudo apt-get install libpq-dev
sudo apt-get install nodejs
sudo apt-get install imagemagick
```
