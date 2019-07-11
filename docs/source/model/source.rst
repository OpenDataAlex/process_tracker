Source
######

Sources are data stores that can either be the source of data to be processed or targets for processes to write data to.

.. _dataset_type_lkup:

Dataset Type
************

This table tracks dataset types (i.e. Customer, Sales, Employee, etc.).

.. list-table:: dataset_type_lkup
   :widths: 25 25 50
   :header-rows: 1

   * - Column Name
     - Column Type
     - Column Description
   * - dataset_type_id
     - Auto incrementing integer sequence
     - System key for the dataset type
   * - dataset_type
     - String(250)
     - Unique name of the given dataset type


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


.. _source_dataset_type:

Source Dataset Type
*******************

This table tracks the relationship between sources and dataset types.

.. list-table:: source_dataset_type
   :widths: 25 25 50
   :header-rows: 1

   * - Column Name
     - Column Type
     - Column Description
   * - source_id
     - Integer
     - Foreign key to the :ref:`source_lkup` table
   * - dataset_type_id
     - Integer
     - Foreign key to the :ref:`dataset_type_lkup` table


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


.. _source_object_dataset_type:

Source Object Dataset Type
**************************

This table tracks the relationship between source/target objects and dataset types.

.. list-table:: source_object_dataset_type
   :widths: 25 25 50
   :header-rows: 1

   * - Column Name
     - Column Type
     - Column Description
   * - source_object_id
     - Integer
     - Foreign key to the :ref:`source_object_lkup` table
   * - dataset_type_id
     - Integer
     - Foreign key to the :ref:`dataset_type_lkup` table