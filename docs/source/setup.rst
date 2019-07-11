Setting Up ProcessTracker
#########################

Before utilizing one of the implementations of ProcessTracker, there is a little bit of setup that must be completed.

Data Store Setup
****************

ProcessTracker utilizes a relational database to store and track all of it's Process and Extract information.  Eventually,
setting up the data store will be enabled in the code implementations of ProcessTracker, but in the meantime, there are
prepared scripts for four of the more popular relational databases (Postgres, MySQL, MS-SQL, Oracle) that can be found
here <add doc link>.

.. _configuration:

Configuration
*************

As far as configuration is concerned, for code package implementations a simple config file (process_tracker.ini) can be
found in the user's home directory under .process_tracker once the module has been invoked the first time.  For tool
implementations, configuration will be handled in the best practice of the tool (i.e. the tool's configuration files,
shared modules, etc.)

process_tracker_config.ini
==========================

The process_tracker_config.ini file contains the following settings:

.. list-table:: process_tracker_config.ini
   :widths: 25 25
   :header-rows: 1

   * - Variable Name
     - Variable Description
   * - log_level
     - Logging level of ProcessTracker
   * - max_concurrent_failures
     - Maximum number of concurrent failures allowed before process is put in status 'on hold'
   * - data_store_type
     - The database type.  Valid options:  postgresql, mysql, mssql, oracle, snowflake
   * - data_store_username
     - The username used to connect to the data store
   * - data_store_password
     - The password used by the username to connect to the data store
   * - data_store_host
     - The host server of the data store
   * - data_store_port
     - The port of the host used to connect to the data store
   * - data_store_name
     - The name of the database/schema storing ProcessTracking data in the data store

The data_store_* variables are used to build a connection string to the data store.

Encrypting Data Store Passwords
===============================

Data store passwords can be encrypted so that they are not stored in plain text. The method used is NOT cryptographically
secure.  This just makes it a bit harder for someone to access your data store password.  Please refer to the
:ref:`password_encryption` section.