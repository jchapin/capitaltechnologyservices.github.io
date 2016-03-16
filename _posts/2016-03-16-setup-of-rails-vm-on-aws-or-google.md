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
    https://aws.amazon.com, register and connect a credit card to the account.
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
        https://aws.amazon.com/ec2/pricing/
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