Overview
********
Below is an overview of how to work with the Pythom implementation of ProcessTracker.

How Does It Work?
`````````````````
The Python ProcessTracker implementation consists of two core classes:  ProcessTracker and ExtractTracker.
Let's start with an example for ProcessTracker.  Consider that we are running a pyspark job::

        from pyspark.sql import SparkSession

        spark = SparkSession.builder.master("local")\
                                    .appName("Lahman Teams Load")\
                                    .config("spark.executor.memory", "6gb")\
                                    .enableHiveSupport()\
                                    .getOrCreate()

This builds a pyspark session.  To register it within ProcessTracker, we need to modify our pyspark job::

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

That will register the process, actor (the person or environment that's doing the processing), tool, and source if they have not been registered before as well as create a new
process run.  If everything works as expected (because we live in a perfect world, right?) the process run can be finished
with a simple command::

        process_run.change_run_status('completed')

This will update the run record to show that it is completed, which will allow dependent processes or new runs of the same
process to run.

Now let's say we're actually working with staged data files - either producing them or using them from another process.
This is where ExtractTracker will come in handy.  Consider that we have the same pyspark process from earlier, which is
loading the Teams.csv file::

        data_set = spark.read.format("csv").option("header", "true") \
                             .load("~/baseballdatabank-master_2018-03-28/baseballdatabank-master/core/Teams.csv")

Now it would be good to know that not only did this process use the Teams.csv file, but what if another process had
already loaded it?  Let's modify our pyspark job to also register the Teams.csv file::

        from processtracker import ExtractTracker, ProcessTracker
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
                                             , source_name='Lahman Baseball Dataset')

        extract = ExtractTracker(process_run=process_run
                                      , filename='Teams.csv'
                                      , location_name='Lahman Baseball Databank 2018'
                                      , location_path='~/baseballdatabank-master_2018-03-28/baseballdatabank-master/core/')

        extract.change_extract_status('loading')

        data_set = spark.read.format("csv").option("header", "true") \
                             .load("~/baseballdatabank-master_2018-03-28/baseballdatabank-master/core/Teams.csv")

Now our Teams.csv is registered if it wasn't already, along with the location where Teams.csv can be found.  The
association between the process run and the extract is also tracked.  Then to show that we are doing something to the
file other than creating it (the extract gets an initial status of initializing), we change it's status to loading.
When we are done with the extract, we simply change it's status again and complete the process run.  Our example pyspark
process now looks like::

        from processtracker import ExtractTracker, ProcessTracker
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
                                             , source_name='Lahman Baseball Dataset')

        extract = ExtractTracker(process_run=process_run
                                      , filename='Teams.csv'
                                      , location_name='Lahman Baseball Databank 2018'
                                      , location_path='~/baseballdatabank-master_2018-03-28/baseballdatabank-master/core/')

        extract.change_extract_status('loading')

        data_set = spark.read.format("csv").option("header", "true") \
                             .load("~/baseballdatabank-master_2018-03-28/baseballdatabank-master/core/Teams.csv")

        <load Teams.csv and do other things>

        extract.change_extract_status('loaded')
        process_run.change_run_status('completed')
