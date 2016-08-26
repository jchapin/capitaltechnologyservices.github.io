---
layout: post
title: "Rails - Converting from Heroku PostgreSQL to Google Cloud SQL (MySQL)"
title_short: "Rails - Converting from PostgreSQL to MySQL"
excerpt: >
  This article covers converting from Heroku PostgreSQL to Google Cloud SQL,
  which uses MySQL for its RDBMS.
---

{{ page.excerpt }}

Most of the time we deploy clients to Heroku, a very slick and easy to use
Platform as a Service (PaaS). But occasionally we run into clients with a
preference for Google Cloud Services. In this specific case, we started with
the client's app deployed on Heroku and Heroku's hosted PostgreSQL database
service. We focused first on deploying the Rails application over to GCS
because there was minimal performance impact to running the database on Heroku's
PostgreSQL service while the app was hosted on Google. But now we're back and
we are going to finish the job with a full conversion over to Google's Cloud SQL
product. That means switching to MySQL. Here's how to do it:

1.  Follow Heroku's instructions for how to backup and then export the latest
    dump of your database to your local machine. At the end of the process you
    will have a file named "latest.dump" that you will have downloaded using
    curl.

    [https://devcenter.heroku.com/articles/heroku-postgres-import-export](https://devcenter.heroku.com/articles/heroku-postgres-import-export)

    We have to process this file a bit in order to import it into MySQL and
    oddly that means having a local instance of PostgreSQL handy to import and
    re-export the data.
1.  Use psql to create a transition_database on the local PostgreSQL server.
    ```CREATE DATABASE transition_database;```
1.  Load the data into a local copy of PostgreSQL:
    ```pg_restore --verbose --clean --no-acl --no-owner -d transition_database latest.dump```
1.  Export the data again as a collection of INSERT queries instead of COPY,
    which is not a statement in the SQL standard.
    ```pg_dump --data-only --column-inserts --file=import.sql transition_database```
1.  Remove all of the SET statements at the top of the import.sql file
1.  Add ```USE name_of_target_database;``` to the top of the import.sql file.
    This may not be as important if you're using the command line client and
    specifying the target database on connection. But MySQLWorkbench complained
    about not knowing which database to run queries against when I was connected
    to the server.
1.  Replace all of the sequence setting statements with alterations of the
    autoincrement values on the table. Note that you have to set the
    autoincrement value to the value of the PostgreSQL sequence + 1.
    ```SELECT pg_catalog.setval('addresses_id_seq', 22, true);```
    ```ALTER TABLE addresses AUTO_INCREMENT=23;```
    This is tedious if you have a large collection of tables, but can be made
    easier by using a regular expression to hunt out and replace lines beginning
    with ```^SELECT```.
1.  Add the "mysql2" gem to your Gemfile.
1.  ```bundle install```
1.  Open your config/database.yml file and add the details of the new MySQL
    connection under your production configuration.
1.  ```rake db:migrate RAILS_ENV=production``` This is going to setup your
    all of the tables necessary, but they will of course be empty.
1.  IF your import is small:
    1.  I used MySQLWorkbench to connect to the Google Cloud SQL instance and
        run import.sql.
    1.  You could just as easily use the command line client to import the data.
        ```mysql -h IP.ADD.OF.SQL -u USERNAME -p database_name < import.sql```
1.  IF your import is large:
    1.  Upload your import.sql file to a Cloud Storage Container
    1.  Go to your database in Google Cloud SQL and import the file by finding
        it in the cloud storage container.
