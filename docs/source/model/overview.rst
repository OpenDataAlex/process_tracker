Overview
********

ProcessTracker officially supports Postgresql and MySQL as fully tested and verified to work.

Support is also enabled for Oracle, MS-SQL, and Snowflake, but has not been fully tested thru the build stack.  While it
should Just Work (tm) until they can be tested thru the build stack and not just locally their status must remain
'available but untested'.

Each subsection of the Model section covers a specific topic of the data store.

New with version 0.9.0, all tables are now equipped with stock audit fields:

.. list-table:: stock audit fields
   :widths: 25 25 50
   :header-rows: 1

   * - Column Name
     - Column Type
     - Column Description
   * - created_date_time
     - timestamp (with timezone, if available)
     - The date/time that the record was created.
   * - created_by
     - integer
     - The id of the actor who created the record. Implied foreign key to the :ref:`actor_lkup` table.
   * - update_date_time
     - timestamp (with timezone, if available)
     - The date/time that the record was last updated.
   * - updated_by
     - integer
     - The id of the actor who updated the record. Implied foreign key to the :ref:`actor_lkup` table.