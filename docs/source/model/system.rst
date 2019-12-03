System Settings
###############

For future updates, system tables are provided.  This is only used by the system.  Anything added to these tables will
be ignored.

.. _system:

System
******

This is the core system table, to be used in future updates.

.. list-table:: system_lkup
   :widths: 25 25 50
   :header-rows: 1

   * - Column Name
     - Column Type
     - Column Description
   * - system_id
     - Auto incrementing integer sequence
     - System key for the system key record
   * - system_key
     - String(250)
     - Unique key record for system value
   * - system_value
     - String(250)
     - Value for the given key.

The following defaults are provided on initialization.

.. list-table:: system_lkup defaults
   :widths: 25 25 50
   :header-rows: 1

   * - System Key
     - System Value
     - Description
   * - version
     - <system version>
     - the version of the ProcessTracking system in use.

No custom system key/value pairs are used.  This is purely for system use.