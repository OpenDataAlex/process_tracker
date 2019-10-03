Source
######

Sources are data stores that can either be the source of data to be processed or targets for processes to write data to.


.. _data_type_lkup:

Data Type
*********

This table tracks the unique data types of source object attributes.

.. list-table:: data_type_lkup
   :widths: 25 25 50
   :header-rows: 1

   * - Column Name
     - Column Type
     - Column Description
   * - data_type_id
     - Integer
     - Auto incrementing unique sequence.
   * - data_type
     - String(75)
     - Unique data type name.



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


.. _filter_type_lkup:

Filter Type
***********

This table tracks the unique filter types used by processes.

.. list-table:: filter_type_lkup
   :widths: 25 25 50
   :header-rows: 1

   * - Column Name
     - Column Type
     - Column Description
   * - filter_type_id
     - Integer
     - Auto incrementing unique sequence
   * - filter_type_code
     - String(3)
     - The unique code for a given filter type.
   * - filter_type_name
     - String(75)
     - The unique filter type name.

Some default filter types are provided on initialization.

.. list-table:: Default Filter Types
   :widths: 25 50
   :header-rows: 1

   * - Filter Type Code
     - Filter Type Name
     - Description
   * - eq
     - equal to
     - Does the attribute equal to a given value?
   * - lt
     - less than
     - Is the attribute less than a given value?
   * - gt
     - greater than
     - Is the attribute greater than a given value?
   * - lte
     - less than or equal to
     - Is the attribute less than or equal to a given value?
   * - gte
     - greater than or equal to
     - Is the attribute greater than or equal to a given value?
   * - not
     - not equal
     - Does the attribute not equal to a given value?
   * - lke
     - like
     - Is the attribute like a given value?
   * - in
     - in set
     - Is the attribute in a given value set?


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


.. _source_contact:

Source Contact
**************

This table tracks the relationship between sources and contacts.

.. list-table:: source_contact
   :widths: 25 25 50
   :header-rows: 1

   * - Column Name
     - Column Type
     - Column Description
   * - source_id
     - Integer
     - The Contact's source system.  Foreign key to :ref:`source_lkup`.
   * - contact_id
     - Integer
     - The Source system's contact(s). Foreign key to :ref:`contact_lkup`.


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


.. _source_object_attribute:

Source Object Attribute
***********************

This is the core table tracking source/target object attributes.

.. list-table:: source_object_attribute
   :widths: 25 25 50
   :header-rows: 1

   * - Column Name
     - Column Type
     - Column Description
   * - source_object_attribute_id
     - Integer
     - Auto incrementing integer sequence.
   * - source_object_attribute_name
     - String(250)
     - Name of the source object attribute.  Must be unique to the source_object.
   * - source_object_id
     - Integer
     - The attribute's source object.  Foreign key to :ref:`source_object_lkup`
   * - attribute_path
     - String(750)
     - For attributes from sources like json, the path to get to the attribute.
   * - data_type_id
     - The data type of the attribute.  Foreign key to :ref:`data_type_lkup`
   * - data_length
     - Integer
     - The length of the attribute.
   * - data_decimal
     - Integer
     - How many decimal places of the attribute.
   * - is_pii
     - Boolean
     - Is the attribute Personally Identifiable Information (PII)?
   * - default_value_string
     - String(250)
     - For string based attributes, the default value.
   * - default_value_number
     - Numeric
     - For numeric based attributes, the default value.


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