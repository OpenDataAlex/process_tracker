Error Tracking
##############

These are the tables associated with error tracking/handling.  Links to other subjects will be provided as noted.

.. _error_tracking:

Error Tracking
**************

This is the core error tracking table.  This associates errors to process runs.

.. list-table:: error_tracking
   :widths: 25 25 50
   :header-rows: 1

   * - Column Name
     - Column Type
     - Column Description
   * - error_tracking_id
     - Auto incrementing integer sequence
     - System key for the error tracking record
   * - error_type_id
     - Integer
     - Foreign key to :ref:`error_type_lkup` lookup table.
   * - error_description
     - String(750)
     - Provided description that further defines the error type.  Not required.
   * - error_occurrence_date_time
     - Datetime/timestamp
     - The date/time that the error occurred
   * - process_tracking_id
     - Integer
     - Foreign key to :ref:`process_tracking` table.


.. _error_type_lkup:

Error Type
**********

Error types can be any user defined label of errors.

.. list-table:: error_type_lkup
   :widths: 25 25 50
   :header-rows: 1

   * - Column Name
     - Column Type
     - Column Description
   * - error_type_id
     - Auto incrementing integer sequence
     - System key for the error type
   * - error_type_name
     - String(250)
     - Unique label for the class of error type

Some default error types are provided on initialization.

.. list-table:: Default Error Types
   :widths: 25 50
   :header-rows: 1

   * - Error Type
     - Description
   * - File Error
     - Errors associated with file problems - opening, writing, reading, missing, etc.
   * - Data Error
     - Errors associated with data problems - format, quality, out of range, etc.
   * - Process Error
     - Errors associated with process problems.

Custom error types can be added as needed to provide finer-grained recording and monitoring.