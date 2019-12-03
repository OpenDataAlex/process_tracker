ExtractTracker - Python
#######################

Below is a deeper dive of the capabilities of the Python implementation ExtractTracker submodule.  Note that
ProcessTracker MUST be used in conjunction with ExtractTracker.

Registering Extracts
********************

Once the process run has been registered, an extract can be registered, provided the following variables are set.::

        process_run = ProcessTracker(process_name='Lahman Teams Load'
                                             , process_type='Stage Load'
                                             , actor_name='New ProcessTracker User'
                                             , tool_name='Spark'
                                             , source_name='Lahman Baseball Dataset')

        extract = ExtractTracker(process_run=process_run
                                      , filename='Teams.csv'
                                      , location_name='Lahman Baseball Databank 2018'
                                      , location_path='~/baseballdatabank-master_2018-03-28/baseballdatabank-master/core/')

Those variables will be used to populate the data store backend as explained in the following table:

.. list-table:: ExtractTracker object initialization variables
   :widths: 25 50 20 10
   :header-rows: 1

   * - Variable Name
     - Variable Description
     - Reference Object
     - Object Created If Not Exist?
   * - process_run
     - An instance of ProcessTracker
     - :ref:`process_tracking`
     - No
   * - filename
     - The extract file's filename
     - :ref:`extract_tracking`
     - Yes
   * - location
     - An instance of Extract Location, optional if created already.
     - :ref:`location_lkup`
     - No
   * - location_name
     - The given name of the location.  Optional.
     - :ref:`location_lkup`
     - Yes
   * - location_path
     - The filepath of the location.  Required if location instance not provided.
     - :ref:`location_lkup`
     - Yes
   * - status
     - The extract file status.  Optional.
     - :ref:`extract_status_lkup`
     - Yes
   * - filetype
     - The name of the extract's filetype.
     - :ref:`extract_filetype_lkup`
   * - compression_type
     - The type of compression used on the extract.  Optional.
     - :ref:`extract_compression_type_lkup`
   * - extract_id
     - When recreating the ExtractTracker instance, provide the extract_id and the instance will be re-instantiated
     - :ref:`extract_tracking`
     - No
   * - file_size
     - The size of the file
     - :ref:`extract_tracking`
     - Yes

Changing Extract Status
***********************

As extract files are used within a process run, their status will need to be modified.::

        extract.change_extract_status(status='loading')

Custom extract status can be entered, but the default status types must be used for ProcessTracker to know what to do
with files.  As long as the file's status is eventually changed to one of those then the process flow will continue.


Extract Dependencies
********************

Extracts can have dependencies between each other, just like processes.  To add dependencies as part of the data
pipeline, use the add_dependency function.::

        extract.add_dependency(dependency_type='parent', dependency=parent_extract)

Child dependencies can also be registered.::

        extract.add_dependency(dependency_type='child', dependency=child_extract)

Extract Audit Information
*************************

Audit information can be collected on individual extracts.

Setting Extract Data's Low and High Dates
-----------------------------------------

If an extract has a date field, it can be used to store the extract file's lowest and highest datetimes.::

        extract.set_extract_low_high_dates(low_date="1900-01-01 00:00:00", high_date="2019-12-31 00:00:00")

Dates can be refreshed as the data is processed, or the low and high dates can be predetermined and passed to
ExtractTracker before finishing the processing of the file by changing it's status.

Two types of dates are tracked:  dates on write and dates on load.  set_extract_low_high_dates has a default type of
'load'.  To change it, just set audit_type.::

        extract.set_extract_low_high_dates(low_date="1900-01-01 00:00:00", high_date="2019-12-31 00:00:00", audit_type='write')

Setting Extract Record Count
----------------------------

If tracking number of records is required, set_extract_record_count will keep the status of the number of records per
file, once it is provided the record count.::

        extract.set_extract_record_count(num_records=792)

As with low and high dates, two types of counts are tracked:  write and load.  set_extract_record_count has a default
type of 'load'.  To change it, just set audit_type.::

        extract.set_extract_record_count(num_records=792, audit_type='write')

Extract Helpers
***************

There are also a few helpers available to ExtractTracker objects to assist in using extracts:

Location
--------

The extract's location object can be retrieved by using:::

        extract = ExtractTracker(process_run=process_run
                                      , filename='Teams.csv'
                                      , location_name='Lahman Baseball Databank 2018'
                                      , location_path='~/baseballdatabank-master_2018-03-28/baseballdatabank-master/core/')

        extract.location # This is the location object associated to the extract.

Attributes of the location can be called by using the attribute's name:::

        extract.location.location_bucket_name # To retrieve the location's bucket (if s3 location)

