Source
######

Sources are data stores that can either be the source of data to be processed or targets for processes to write data to.

.. _source_lkup:

Source
******

This is the core table tracking sources/targets.

.. list-table:: source_lkup
   :widths: 25 25 50
   :header-rows: 1

   * - Column Name
     - Column Type
     - Column Description
   * - source_id
     - Auto incrementing integer sequence
     - System key for the source
   * - source_name
     - String(250)
     - Unique name of the given source.


.. _source_object_lkup:

Source Object
*************

This is the core table tracking source/target objects.

.. list-table:: source_object_lkup
   :widths: 25 25 50
   :header-rows: 1

   * - Column Name
     - Column Type
     - Column Description
   * - source_object_id
     - Auto incrementing integer sequence
     - System key for the source object
   * - source_id
     - Foreign key to :ref:`source_lkup`.
   * - source_object_name
     - String(250)
     - Unique object name from given source.