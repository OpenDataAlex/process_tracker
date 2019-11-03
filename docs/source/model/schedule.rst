Schedule
########

These are the tables associated with process schedules.  Links to other subjects will be provided as noted.

.. _schedule_frequency_lkup:

ScheduleFrequency
*****************

This table tracks general schedule frequencies.

.. list-table:: schedule_frequency_lkup
   :widths: 25 25 50
   :header-rows: 1

   * - Column Name
     - Column Type
     - Column Description
   * - schedule_frequency_id
     - Integer
     - Auto incrementing interger sequence.
   * - schedule_frequency_name
     - String(25)
     - Unique name of the schedule frequency.

Some default schedule frequencies are provided on initialization.

.. list-table:: Default Schedule Frequencies
   :widths: 25 50
   :header-rows: 1

   * - Schedule Frequency
     - Description
   * - unscheduled
     - Processes are not run on any schedule.
   * - hourly
     - Processes should run on an hourly basis.
   * - daily
     - Processes should run on a daily basis.
   * - weekly
     - Processes should run on a weekly basis.
   * - monthly
     - Processes should run on a monthly basis.
   * - quarterly
     - Processes should run on a quarterly basis.
   * - annually
     - Processes should run on an annual basis.