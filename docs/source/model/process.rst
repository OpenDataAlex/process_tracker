Process Tracking
################

These are the tables associated with process tracking/handling.  Links to other subjects will be provided as noted.

.. _process:

Process
*******

This table tracks the unique processes being tracked by ProcessTracker.

.. list-table:: process
   :widths: 25 25 50
   :header-rows: 1

   * - Column Name
     - Column Type
     - Column Description
   * - process_id
     - Auto incrementing integer sequence
     - System key for the process
   * - process_name
     - String(250)
     - Unique name of the process
   * - total_record_count
     - Integer
     - Audit field that tracks the total number of records processed throughout the lifetime of the process.
   * - process_type_id
     - Integer
     - The type of process.  Foreign key to :ref:`process_type_lkup`.
   * - process_tool_id
     - Integer
     - The type of tool used to run the process.  Foreign key to :ref:`tool_lkup`.
   * - last_failed_run_date_time
     - Datetime/timestamp
     - The date/time of the last failed run of this process.
   * - schedule_frequency_id
     - Integer
     - The schedule frequency of the process.  Foreign key to :ref:`schedule_frequency_lkup`
   * - last_completed_run_date_time
     - Datetime/timestamp
     - The date/time of the last successful completion of this process
   * - last_errored_run_date_time
     - Datetime/timestamp
     - The date/time of the last errored run of this process


.. _process_contact:

Process Contact
***************

This table tracks the relationship between processes and their contacts.

.. list-table:: process_contact
   :widths: 25 25 50
   :header-rows: 1

   * - Column Name
     - Column Type
     - Column Description
   * - process_id
     - Integer
     - The contact's process.  Foreign key to :ref:`process`.
   * - contact_id
     - Integer
     - The process' contact.  Foreign key to :ref:`contact`.


.. _process_dataset_type:

Process Dataset Type
********************

This table tracks the relationship between process and dataset types.

.. list-table:: process_dataset_type
   :widths: 25 25 50
   :header-rows: 1

   * - Column Name
     - Column Type
     - Column Description
   * - process_id
     - Integer
     - Foreign key to the :ref:`process` table.
   * - dataset_type_id
     - Integer
     - Foreign key to the :ref:`dataset_type_lkup` table.


.. _process_dependency:

Process Dependency
******************

This table tracks the interdependencies between processes, regardless of tool/method to execute said processes.

.. list-table:: process_dependency
   :widths: 25 25 50
   :header-rows: 1

   * - Column Name
     - Column Type
     - Column Description
   * - parent_process_id
     - Integer
     - The parent process of the parent-child relationship.  Foreign key to :ref:`process`.
   * - child_process_id
     - Integer
     - The child process of the parent-child relationship.  Foreign key to :ref:`process`.

Please note - the dependency hierarchy can theoretically go on infinitely.  In reality only a few levels either way
would realistically be used, but this type of relationship can cause performance issues.


.. _process_filter:

Process Filter
**************

This table tracks query filters for a given process.

.. list-table:: process_filter
   :widths: 25 25 50
   :header-rows: 1

   * - Column Name
     - Column Type
     - Column Description
   * - process_filter_id
     - Integer
     - Auto incrementing unique sequence
   * - process_id
     - Integer
     - The filter's process.  Foreign key to :ref:`process`.
   * - source_object_attribute_id
     - Integer
     - The process filter's source_object attribute.  Foreign key to :ref:`source_object_attribute`.
   * - filter_type_id
     - Integer
     - The filter's type.  Foreign key to :ref:`filter_type_lkup`
   * - filter_value_string
     - String(250)
     - For character based attributes, the string comparator.
   * - filter_value_numeric
     - Numeric
     - For numeric based attributes, the numeric comparator.

.. _process_source:

Process Source
**************

This table tracks what sources are used by a given process.

.. list-table:: process_source
   :widths: 25 25 50
   :header-rows: 1

   * - Column Name
     - Column Type
     - Column Description
   * - source_id
     - Integer
     - The source system utilized by the process.  Foreign key to :ref:`source_lkup`.
   * - process_id
     - Integer
     - The process utilizing the source.  Foreign key to :ref:`process`.


.. _process_source_object:

Process Source Object
*********************

This table tracks the finer grained relationship between process and source object.


.. list-table:: process_source_object
   :widths: 25 25 50
   :header-rows: 1

   * - Column Name
     - Column Type
     - Column Description
   * - process_id
     - Integer
     - The process utilizing the source object.  Foreign key to :ref:`process`.
   * - source_object_id
     - Integer
     - The source object being utilized by the process.  Foreign key to :ref:`source_object_lkup`.


.. _process_source_object_attribute:

Process Source Object Attribute
*******************************

This table tracks even finer grained relationships between process and source object attributes.


.. list-table:: process_source_object_attribute
   :widths: 25 25 50
   :header-rows: 1

   * - Column Name
     - Column Type
     - Column Description
   * - process_id
     - Integer
     - The Process associated to the source object attribute.  Foreign key to :ref:`process`.
   * - source_object_attribute_id
     - Integer
     - The Source Object Attribute associated to the process.  Foreign key to :ref:`source_object_attribute`.
   * - source_object_attribute_alias
     - String(250)
     - The optional alias used by the process on the attribute.
   * - source_object_attribute_expression
     - String(250)
     - The optional expression (calculation) used on the attribute.


.. _process_status_lkup:

Process Status
**************

This table is a lookup table for the types of process statuses available in the system.

.. list-table:: process_status_lkup
   :widths: 25 25 50
   :header-rows: 1

   * - Column Name
     - Column Type
     - Column Description
   * - process_status_id
     - Auto incrementing integer sequence
     - System key for the process
   * - process_status_name
     - String(75)
     - Unique name of the process status

Some default process status types are provided on initialization.

.. list-table:: Default Process Status Types
   :widths: 25 50
   :header-rows: 1

   * - Process Status Type
     - Description
   * - running
     - The process is running.  No other instances or child dependencies can be run.
   * - completed
     - The process completed successfully.  Other instances and child dependencies can be run.
   * - failed
     - The process did not complete successfully.  Other instances may be run, but child dependencies will be blocked.

Other custom process status types can be added, but the system can not currently take advantage of them.

.. _process_target:

Process Target
**************

This table tracks the targets that processes write to.  Target is an alias of source since sources can be targets and
vice-versa.

.. list-table:: process_target
   :widths: 25 25 50
   :header-rows: 1

   * - Column Name
     - Column Type
     - Column Description
   * - target_source_id
     - Integer
     - The source system the process is writing to.  Foreign key to :ref:`source_lkup`.
   * - process_id
     - Integer
     - the process utilizing the source.  Foreign key to :ref:`process`.


.. _process_target_object:

Process Target Object
*********************

This table tracks the finer grained relationship between process and source target object.


.. list-table:: process_target_object
   :widths: 25 25 50
   :header-rows: 1

   * - Column Name
     - Column Type
     - Column Description
   * - process_id
     - Integer
     - The process utilizing the source object.  Foreign key to :ref:`process`.
   * - target_object_id
     - Integer
     - The target object being utilized by the process.  Foreign key to :ref:`source_object_lkup`.


.. _process_target_object_attribute:

Process Target Object Attribute
*******************************

This table tracks even finer grained relationships between process and target source object attributes.


.. list-table:: process_target_object_attributes
   :widths: 25 25 50
   :header-rows: 1

   * - Column Name
     - Column Type
     - Column Description
   * - process_id
     - Integer
     - The Process associated to the target source object attribute.  Foreign key to :ref:`process`.
   * - target_object_attribute_id
     - Integer
     - The Target Source Object Attribute associated to the process.  Foreign key to :ref:`source_object_attribute`.
   * - target_object_attribute_alias
     - String(250)
     - The optional alias used by the process on the attribute.
   * - target_object_attribute_expression
     - String(250)
     - The optional expression (calculation) used on the attribute.


.. _process_tracking:

Process Tracking
****************

This table is the core of the process tracking subsystem.

.. list-table:: process_tracking
   :widths: 25 25 50
   :header-rows: 1

   * - Column Name
     - Column Type
     - Column Description
   * - process_tracking_id
     - Auto incrementing integer sequence
     - System key for the process run
   * - process_id
     - Integer
     - The process being run.  Foreign key to :ref:`process`.
   * - process_status_id
     - Integer
     - The current status of the process run.  Foreign key to :ref:`process_status_lkup`.
   * - process_run_id
     - Integer
     - Unique sequence of the given process' runs.
   * - process_run_low_date_time
     - Datetime
     - The earliest derived datetime for data processed in this process run.  Optional audit field.
   * - process_run_high_date_time
     - Datetime
     - The latest derived datetime for data processed in this process run.  Optional audit field.
   * - process_run_start_date_time
     - Datetime/timestamp
     - The date/time that the process run was registered.
   * - process_run_end_date_time
     - Datetime/timestamp
     - The date/time that the process finished running, regardless of success or failure.
   * - process_run_record_count
     - Integer
     - For the given process run, the total number of records processed.  Optional audit field.
   * - process_run_actor_id
     - Integer
     - The person or thing that kicked off the process run.  Foreign key to :ref:`actor_lkup`.
   * - is_latest_run
     - Boolean
     - Bit to determine if for the given process if the record is the latest run or not.
   * - process_run_name
     - String(250)
     - Unique process instance name, optional.


.. _process_type_lkup:

Process Type
************

This table is a lookup of the various process types available.

.. list-table:: process_type_lkup
   :widths: 25 25 50
   :header-rows: 1

   * - Column Name
     - Column Type
     - Column Description
   * - process_type_id
     - Auto incrementing integer sequence
     - System key for the process type
   * - process_type_name
     - String(250)
     - Unique name of the process type

Some default process types are provided on initialization.

.. list-table:: Default Process Types
   :widths: 25 50
   :header-rows: 1

   * - Process Type
     - Description
   * - Extract
     - Process that is focused on extracting data.
   * - Load
     - Process that is focused on loading data.

Custom process types can be added.