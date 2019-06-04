Extract Tracker
###############

These are the tables associated with extract tracking/handling.  Links to other subjects will be provided as noted.

.. _extract_process_tracking:

Extract Process Tracking
************************

This table tracks the association between extract files and process runs.


.. list-table:: extract_process_tracking
   :widths: 25 25 50
   :header-rows: 1

   * - Column Name
     - Column Type
     - Column Description
   * - extract_tracking_id
     - Integer
     - Foreign key to the :ref:`extract_tracking` table.
   * - process_tracking_id
     - Integer
     - Foreign key to the :ref:`process_tracking` table.
   * - extract_process_status_id
     - Integer
     - Status of the extract from the process run.  Foreign key to the :ref:`extract_status_lkup` table.
   * - extract_process_event_date_time
     - Datetime/timestamp
     - The date/time of the status change for the extract.

.. _extract_status_lkup:

Extract Status
**************

This table is a lookup of system and user provided extract statuses.

.. list-table:: extract_status_lkup
   :widths: 25 25 50
   :header-rows: 1

   * - Column Name
     - Column Type
     - Column Description
   * - extract_status_id
     - Auto incrementing integer sequence
     - System key for the extract status
   * - extract_status_name
     - String(75)
     - Unique name of the extract status type

Some default extract status types are provided on initialization.

.. list-table:: Default Extract Status Types
   :widths: 25 50
   :header-rows: 1

   * - Extract Status Type
     - Description
   * - initializing
     - The extract file is being written to and/or is not ready for use.
   * - ready
     - The extract file is ready to be used.
   * - loading
     - The extract file is being used/loaded by a process run.
   * - loaded
     - The extract file has successfully been loaded by a process run.
   * - archived
     - The extract file has successfully been archived and can only be reprocessed if moved back out of archive location.
   * - deleted
     - The extract file has successfully been removed from the archive and can no longer be retrieved.
   * - error
     - Something went wrong in the writing/processing of the extract file.  Until resolved, file is unusable.

Custom extract status types can be added, but can not currently be utilized by the ProcessTracker framework.

.. _extract_tracking:

Extract Tracking
****************

This table is the core of the extract tracking subsystem.

.. list-table:: extract_tracking
   :widths: 25 25 50
   :header-rows: 1

   * - Column Name
     - Column Type
     - Column Description
   * - extract_id
     - Auto incrementing integer sequence
     - System key for the extract file
   * - extract_filename
     - String(750)
     - The unique filename of the extract file
   * - extract_location_id
     - Integer
     - Where the extract file can be located.  Foreign key to :ref:`location_lkup`
   * - extract_status_id
     - Integer
     - The current status of the extract file.  Foreign key to :ref:`extract_status_lkup`
   * - extract_registration_date_time
     - Datetime/timestamp
     - The date/time that the extract was initially registered into the system.


.. _location_lkup:

Location
********

This table tracks extract file locations.

.. list-table:: location_lkup
   :widths: 25 25 50
   :header-rows: 1

   * - Column Name
     - Column Type
     - Column Description
   * - location_id
     - Auto incrementing integer sequence
     - System key for the file location
   * - location_name
     - String(750)
     - Unique optional name of the location.  Will be derived from the filepath if not provided.
   * - location_path
     - String(750)
     - Unique filepath.
   * - location_type_id
     - Integer
     - The type of location for given filepath.  Foreign key to :ref:`location_type_lkup`.

.. _location_type_lkup:

Location Type
*************

This table tracks extract file location types.

.. list-table:: location_type_lkup
   :widths: 25 25 50
   :header-rows: 1

   * - Column Name
     - Column Type
     - Column Description
   * - location_type_id
     - Auto incrementing integer sequence
     - System key for the location type
   * - location_type_name
     - String(25)
     - The unique name of the type of location.

Some default location types are provided on initialization.

.. list-table:: Default Location Types
   :widths: 25 50
   :header-rows: 1

   * - Location Type
     - Description
   * - S3
     - S3 bucket location
   * - Local Filesystem
     - Local filesystem location
