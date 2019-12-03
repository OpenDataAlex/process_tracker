Capacity Management
###################

Capacity Management involves organization of environment resources and assigning processes to specific groups ('clusters').

.. _cluster_tracking:

Cluster
*******

.. list-table:: cluster_tracking_lkup
   :widths: 25 25 50
   :header-rows: 1

   * - Column Name
     - Column Type
     - Column Description
   * - cluster_id
     - Auto incrementing integer sequence
     - System key for the cluster
   * - cluster_name
     - String(250)
     - Unique name of the given cluster
   * - cluster_max_memory
     - Integer
     - The maximum amount of memory allocated to the cluster.  Optional.
   * - cluster_max_memory_unit
     - String(2)
     - The unit of measure for the max memory field.  Examples include:  MB, GB, TB.  Optional.
   * - cluster_max_processing
     - Integer
     - The maximum amount of processing allocated to the cluster.  Optional.
   * - cluster_max_processing_unit
     - Integer
     - The unit of measure for the max processing field.  Examples include: Ghz, CPU, DPU.  Optional.
   * - cluster_current_memory_usage
     - Integer
     - Based on the processes running within the cluster, how much memory are they using.  Optional field.  Not currently in use.
   * - cluster_current_process_usage
     - Integer
     - Based on the processes running within the cluster, how much processing power are they using.  Optional.  Not currently in use.

.. _cluster_process:

ClusterProcess
**************

This table tracks the relationships between clusters and processes.  Used for performance balancing.

.. list-table:: cluster_process
   :widths: 25 25 50
   :header-rows: 1

   * - Column Name
     - Column Type
     - Column Description
   * - cluster_id
     - Integer
     - The cluster of the cluster-process relationship.  Foreign key to :ref:`cluster_tracking_lkup`.
   * - process_id
     - Integer
     - The process of the cluster-process relationship.  Foreign key to :ref:`process`.
