ProcessTracker - Python
#######################

Below is a deeper dive of the capabilities of the Python implementation ProcessTracker submodule.

Initialize Process Run
**********************

As referenced in the ::doc::`Overview <overview.rst>`, ProcessTracker must first be imported and then the process can be
registered by setting the variables of the ProcessTracker object::

        from processtracker import ProcessTracker
        from pyspark.sql import SparkSession

        spark = SparkSession.builder.master("local")\
                                    .appName("Lahman Teams Load")\
                                    .config("spark.executor.memory", "6gb")\
                                    .enableHiveSupport()\
                                    .getOrCreate()

        process_run = ProcessTracker(process_name='Lahman Teams Load'
                                             , process_type='Stage Load'
                                             , actor_name='New ProcessTracker User'
                                             , tool_name='Spark'
                                             , sources='Lahman Baseball Dataset')

Those variables will be used to populate the data store backend as explained in the following table:

.. list-table:: ProcessTracker object initialization variables
   :widths: 25 50 20 10
   :header-rows: 1

   * - Variable Name
     - Variable Description
     - Reference Object
     - Object Created If Not Exist?
   * - process_name
     - The name of the process being run.  Must be unique.
     - :ref:`process`
     - Yes
   * - process_run_name
     - An optional unique name given to an individual process run
     - :ref:`process_tracking`
     - Yes
   * - process_type
     - The type of process being run. Optional if process already exists.
     - :ref:`process_type_lkup`
     - Yes
   * - actor_name
     - The person or thing kicking off the process run.  Recommended to be variable driven.
     - :ref`actor_lkup`
     - Yes
   * - tool_name
     - The tool being used to kick off the process run.  Recommended to be variable driven.
     - :ref:`tool_lkup`
     - Yes
   * - sources
     - Single or list of source names where data is read from.  Optional.
     - :ref:`source_lkup`
     - Yes
   * - source_objects
     - Dictionary of lists of source objects with source name as key where data is read from.  Optional.
     - :ref:`source_object_lkup`, :ref:`process_source_object`
     - Yes
   * - source_object_attributes
     - Dictionary of lists of source object attributes with source and source object names as keys where data is read from. Optional.
     - :ref:`source_object_attribute`
     - Yes
   * - targets
     - Single or list of target names (alias of source) where data is written to. Optional.
     - :ref:`source_lkup`
     - Yes
   * - target_objects
     - Dictionary of lists of target source_objects with target name as key where data is read from.  Optional.
     - :ref:`source_object_lkup`, :ref:`process_target_object`
     - Yes
   * - target_object_attributes
     - Dictionary of lists of target object attributes with target and target object names as keys where data is read from. Optional.
     - :ref:`source_object_attribute`
     - Yes
   * - config_location
     - Location where Process Tracker configuration file can be found.  If not set will use the system home directory
       and check under .process_tracker for the process_tracker_config.ini file. Optional.
     - :ref:`configuration`
     - N/A
   * - dataset_types
     - Single or list of dataset category types.  Will be associated with the process as well as any extracts,
       sources/targets, and source/target objects that are associated with the process.
     - :ref:`dataset_type_lkup`, :ref:`extract_dataset_type`, :ref:`process_dataset_type`, :ref:`source_dataset_type`
       , :ref:`source_object_dataset_type`
     - No
   * - schedule_frequency
     - The general frequency at which the process should run.
     - :ref:`schedule_frequency_lkup`
     - No
   * - process_tracking_id
     - When recreating a Process Tracking instance, provide the process_tracking_id and it will re-instantiate as it was
       originally created, with the current status, objects, etc.
     - :ref:`process_tracking`, plus all other objects created on instantiation of ProcessTracker
     - No

Once the instance has been instantiated, the rest of the options listed below become available.

Re-initializing An Instance
***************************

To set up a previous instance, pass the process tracking id to ProcessTracker.  Instead of creating a new instance of
the given process, it will retrieve the specific tracking record and all of it's ancillary data.::

        restored_process_run = ProcessTracker(process_tracking_id=123)

This should ideally only be used when a process is still running - like when switching between services within your
cloud provider.

Change Process Run Status
*************************

Throughout the process run the process run's status will need to be changed, usually to successful completion or to
failure.  ProcessTracker does allow for user defined process statuses, but the process run must finish with one of the
system provided statuses if the process run is to work correctly with the rest of the system.

System provided statuses can be found at :ref:`process_status_lkup`.::

        process_run.change_run_status('completed')

Custom status types can be added either with the :doc:`CLI </python/cli>` tool or by entering the custom status in the
change_run_status command.  For instance::

        process_run.change_run_status('my custom status')

On Hold Processes
-----------------

Processes that are in the 'on hold' status are in that status because the max_concurrent_failures setting was reached.
Currently, the only way to move a process out of 'on hold' is to manually change the last process run to 'completed' or
some other status than 'running' or 'failed'.

Triggering Errors
*****************

Errors are custom failure messages that can be pretty much anything one would want to track during a process run.  They
do not necessarily trigger a process run to fail.::

        process_run.raise_run_error(error_type_name='Data Error'
                                   , error_description='Data item out of bounds.')

This raises an error stating an item was out of bounds for what we normally look for, but doesn't trigger the process
run to fail because the hidden flag fail_run is defaulted to false.  To fail a run set the flag to True.::

        process_run.raise_run_error(error_type_name='Data Error'
                                   , error_description='Data item out of bounds.'
                                   , fail_run=True)

Another option for raising a run error is to set an end_date - this is if you want tighter control of the timestamps
between ProcessTracker and any other logging you may have.  This is not required because we are ideally talking about
milliseconds between recording this error and writing to the log file.::

        process_run.raise_run_error(error_type_name='Data Error'
                                   , error_description='Data item out of bounds.'
                                   , fail_run=True
                                   , end_date=process_specific_datettime)

Auditing Processes
******************

Auditing is a key feature of the ProcessTracker framework.  Here are the available auditing options.

Setting Data Low/High Dates
---------------------------

It is important to know the data range of the data that is being processed by a run.  This is
where the low/high dates comes to play.  The low date is the lowest date available from the data being processed.  The
high date is the highest date avilable from the data being processed.  If audit dates are not provided with the data then
the extract datetime can be utilized.  If neither are available, then this audit option can't really be used.::

        process_run.set_process_run_low_high_dates(low_date=extract_low_datetime
                                                  , high_date=extract_high_datetime)

If a lower or higher datetime is registered, the previous datetimes will be compared and whichever is lower of the two
low dates and higher of the two high dates will be kept.  While this can be set via loop, it is recommended to find the
low and high dates in the set before calling set_process_run_low_high_dates() as it does make a insert/update per call.

Setting Record Count
--------------------

It is important to know how many records the process and process run have processed.  This can aid capacity and resource
planning, especially if the information is tracked over time.::

        process_run.set_process_run_record_count(num_records=10000)

set_process_run_record_count does two things:
    * sets the process run record's total record count (wiping out the previous
value)
    * sets the process' total record count (cumulative)

It is recommended that the number of records be determined on a per extract file or a cumulative total before setting
the record count.

Dataset Types
*************

Dataset types are high level categories for the type(s) of data the process and ancilliary objects like extracts,
sources/targets, source objects/target objects, etc. can be associated to.  Once the variable dataset_types is set there
is nothing else required to associate the process and other entities associated to the process to the dataset type(s).

Tracking Process Sources
************************

Processes can have sources associated for auditing purposes.  There are two methods for tracking sources - source level
and source object level.

Source Level
------------
Source level tracking can be done by including a single source name or list of source names on process initialization.  For example:::

                process_run = ProcessTracker(process_name='Lahman Teams Load'
                                             , process_type='Stage Load'
                                             , actor_name='New ProcessTracker User'
                                             , tool_name='Spark'
                                             , sources='Lahman Baseball Dataset')

For multiple sources:::

                process_run = ProcessTracker(process_name='Lahman Teams Load'
                                             , process_type='Stage Load'
                                             , actor_name='New ProcessTracker User'
                                             , tool_name='Spark'
                                             , sources={'Lahman Baseball Dataset', 'Another Baseball Dataset'})

Source Object Level
-------------------

Source Object level tracking is done in a similar way as above.  Regardless of being a single source object, multiple
source objects, or multiple sources with single or multiple objects, source object level tracking is done via a dictionary
of lists.::

                process_run = ProcessTracker(process_name='Lahman Teams Load'
                                             , process_type='Stage Load'
                                             , actor_name='New ProcessTracker User'
                                             , tool_name='Spark'
                                             , source_objects={"Lahman Baseball Dataset": ["Team.csv", "Player.csv"]}

For multiple sources:::

                process_run = ProcessTracker(process_name='Lahman Teams Load'
                                             , process_type='Stage Load'
                                             , actor_name='New ProcessTracker User'
                                             , tool_name='Spark'
                                             , source_objects={"Lahman Baseball Dataset": ["Team.csv", "Player.csv"]
                                                               , "Another Baseball Dataset": ["Team", "Player"]}

Note that sources is not set.  The sources variable will be ignored if source_objects is set.

Tracking Process Targets
************************


Processes can have targets associated for auditing purposes.  There are two methods for tracking targets - target level
and target object level.  Remember target is just an alias of source.  All targets and sources are stored in the :ref:`source_lkup` table.

Target Level
------------
Target level tracking can be done by including a single target name or list of target names on process initialization.  For example:::

                process_run = ProcessTracker(process_name='Lahman Teams Load'
                                             , process_type='Stage Load'
                                             , actor_name='New ProcessTracker User'
                                             , tool_name='Spark'
                                             , targets='My Baseball Datastore')

For multiple targets:::

                process_run = ProcessTracker(process_name='Lahman Teams Load'
                                             , process_type='Stage Load'
                                             , actor_name='New ProcessTracker User'
                                             , tool_name='Spark'
                                             , targets={'My Baseball Datastore', 'A Different Baseball Datastore'})

Target Object Level
-------------------

Target Object level tracking is done in a similar way as above.  Regardless of being a single target object, multiple
target objects, or multiple targets with single or multiple targets, target object level tracking is done via a dictionary
of lists.::

                process_run = ProcessTracker(process_name='Lahman Teams Load'
                                             , process_type='Stage Load'
                                             , actor_name='New ProcessTracker User'
                                             , tool_name='Spark'
                                             , target_objects={"My Baseball Datastore": ["team", "player"]}

For multiple targets:::

                process_run = ProcessTracker(process_name='Lahman Teams Load'
                                             , process_type='Stage Load'
                                             , actor_name='New ProcessTracker User'
                                             , tool_name='Spark'
                                             , target_objects={"My Baseball Datastore": ["team", "player"]
                                                               , "A Different Baseball Datastore": ["Team", "Player"]}

Note that targets is not set.  The targets variable will be ignored if target_objects is set.

Process Extracts
****************

The other element to processing data is the extract files that may be used in the process or between processes.  Note
that using this is not required if extract files are not used.  Extracts are always associated with a process run,
which is why the extract functionality is primarily tied to the ProcessTracker submodule.

Finding Extracts
----------------

Extract files can be found in a few different ways.  Finders will return extracts in 'ready' state by default.  Other
statuses can be searched for if required by adding the `status` variable.  The finders also will only return extract
files that have been registered in ProcessTracker.

By Filename
^^^^^^^^^^^

Full Filename
"""""""""""""

So let's say that you know that there is a specific file that needs processing.  You can search for a specific file by::

        process_run.find_extracts_by_filename(filename='my_file.csv')

This will return the ExtractTracking object, which includes the location of the file.

Partial Filename
""""""""""""""""

Let's say that you know that the files you are looking for match a specific pattern, for example:::

        my_file_2019_01_01.csv
        my_file_2019_02_01.csv
        ...

Instead of looking for each file one at a time, you can use the partial filename:::

        process_run.find_extracts_by_filename(filename='my_file_')

This will return the ExtractTracking object, which included the location of the file.  This function is greedy meaning
it will return ANY files with 'my_file' in the filename.  For instance:::

        my_file_2019_01_01.csv
        this_is_my_file.xls
        2019-01-01-my_file.csv

By Location
^^^^^^^^^^^

Locations are the filepaths where extract files are stored.  These can be local, a network drive, or a cloud directory.::

        process_run.find_extracts_by_location(location='My Location')

The location name is used and the ExtractTracking object(s) are returned.

By Process
^^^^^^^^^^

If the process has a parent process that creates files for it, or there is a process that produces files that will be
used then the parent process' name can be used to find any ready extracts:::

        process_run.find_extracts_by_process(extract_process_name='My Super Cool Process')

This will find all extract files associated to that process that are in 'ready' state and return their ExtractTracking
objects.

Finding Extracts By Other Statuses
----------------------------------

All finder methods have a status variable with a default of 'ready'.  To search by another status type, just modify the
variable:::

        process_run.find_extracts_by_location(location='My Location', status='completed')

The status type must exist in :ref:`extract_status_lkup`.

Registering Extracts
--------------------

If your process is creating extract files, they will need to be registered.  They can either be registered one at a time
as noted in :doc:`ExtractTracker </python/extract_tracker>` or one of the below helper methods.

By Location
^^^^^^^^^^^

This will attempt to access the given location and find all files stored there.  If the files are not already registered
they will be processed, otherwise they will be ignored.::

        process_run.register_extracts_by_location(location_path='/path/to/files')

Currently, this only supports local filepaths.

By Process
^^^^^^^^^^

This method is explained over in :doc:`ExtractTracker </python/extract_tracker>`.

Bulk Extract Update
-------------------

Extracts can also be processed in bulk.  If you use one of the lookup functions, it returns a list of extract file objects.
Passing that list to the bulk_change_extract_status method will associate those extracts with the process and bulk update
their status.::

        process_run.bulk_change_extract_status(extracts=extract_list, extract_status="loading")

Please note, that while going through the list if any of the extracts are interdependent of each other and the parent
dependency has not been loaded, the process will currently fail to protect data continuity.

Process Helpers
***************

There are several helpers for ProcessTracker objects to assist in working with other components associated with the
ProcessTracker instance.  Those objects are:

* actor
* process_type
* process
* sources
* targets
* tool

To use attributes of the objects, call the attribute like so:::

        process_run = ProcessTracker(process_name='Lahman Teams Load'
                                             , process_type='Stage Load'
                                             , actor_name='New ProcessTracker User'
                                             , tool_name='Spark'
                                             , sources='Lahman Baseball Dataset')

        process_run.actor.actor_name # To get the actor_name attribute of actor object associated with process_run.


Finding Process Contacts
************************

To find a process' contacts as well as that process' source contacts, there is a lookup function that will return the
name and email of the contact as well as if the contact is for the process itself or that process' source.::

        process_run.find_process_contacts(process_id)


Finding Process Source Attributes
*********************************

Processes usually have sources that they work with to pull data out into either extracts or for further processing.  To
find the source attributes that a process will use there is a helper function.::

        process_run.find_process_source_attributes(process_id)

This will return a list with the following:

.. list-table:: process_source_attributes returned
   :widths: 25 50
   :header-rows: 1

   * - Attribute Name
     - Attribute Description
   * - source_name
     - The name of the source system
   * - source_type
     - The type of the source system
   * - source_object_name
     - The object (i.e. table or dataset) name from the source system
   * - source_object_attribute_name
     - The attribute (i.e. column) name from the source object
   * - is_key
     - Is the attribute part of the source object's key?
   * - is_filter
     - Is the attribute used for record version comparison?
   * - is_partition
     - Is the attribute used for the partitioning of the data set (i.e. if the filetype is Parquet is the attribute used
       for the file partition)?

Please note, if the attributes are not registered either during the initialization of the process or thru direct interaction
of the data store, no data will be returned.

Finding Process Target Attributes
*********************************

Processes usually have targets that they work with to push data into or for further processing.  To find the target
attributes that a process will use there is a helper function.::

        process_run.find_process_target_attributes(process_id)

This will return a list with the following:

.. list-table:: process_target_attributes returned
   :widths: 25 50
   :header-rows: 1

   * - Attribute Name
     - Attribute Description
   * - target_name
     - The name of the target source system
   * - target_type
     - The type of the target source system
   * - target_object_name
     - The object (i.e. table or dataset) name from the target source system
   * - target_object_attribute_name
     - The attribute (i.e. column) name from the target source object
   * - is_key
     - Is the attribute part of the source object's key?
   * - is_filter
     - Is the attribute used for record version comparison?
   * - is_partition
     - Is the attribute used for the partitioning of the data set (i.e. if the filetype is Parquet is the attribute used
       for the file partition)?
Please note, if the attributes are not registered either during hte initialization of the process or thru direct interaction
of the data store, no data will be returned.

Finding Processes By Schedule Frequency
***************************************

Processes can have an optional schedule frequency.  To find all processes of a given frequency there is a helper function.::

        process_run.find_process_by_schedule_frequency(frequency="hourly")

This will return a list of process objects with the given frequency.

Finding Process Filters
***********************

Processes will be querying data from given sources.  The source data will likely be filtered on specific criteria.  To
retrieve the process' filters, use the find_process_filters method.::

        process_run.find_process_filters(process=process_id)

This will return a list of attributes and their requisite filters.

.. list-table:: filter attributes provided
   :widths: 25 50
   :header-rows: 1

   * - Attribute name
     - Attribute description
   * - source_name
     - The name of the source system
   * - source_object_name
     - The name of the object (i.e. table or data set) within/from the source system
   * - source_object_attribute_name
     - The attribute (i.e. column) name from the object in within/from the source system
   * - filter_type_code
     - Based on the defaults in :ref:`filter_type_lkup`, the type of filter being applied to the source object attribute
   * - filter_value_numeric
     - The value being filtered by, provided the attribute is numeric.
   * - filter_value_string
     - The value being filtered by, provided the attribute is string.