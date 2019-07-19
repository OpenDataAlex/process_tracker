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


Changing Extract Status
***********************

As extract files are used within a process run, their status will need to be modified.::

        extract.change_extract_status(status='loading')

Custom extract status can be entered, but the default status types must be used for ProcessTracker to know what to do
with files.  As long as the file's status is eventually changed to one of those then the process flow will continue.

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
