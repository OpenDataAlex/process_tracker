Command Line Tool
#################

The command line tool is designed to help with adding lookup values as well as work with the data store backend.  Once
the processtracker package is installed, the following options will be available from the command line.


Lookup Objects
**************

The following lookup objects can be modified or created via the command line:

* Actor
* Error Type
* Extract Status
* Process Status
* Process Type
* Source
* Tool

Please note, any values added during initialization will not be able to be dropped or modified.

Create
------

This allows for new lookup records to be added.::

        processtracker create --topic Actor --name 'New Actor'

Shorthand can also be used:::

        processtracker create -t Actor -n 'New Actor'

Update
------

This allows for existing lookup records to be modified.::

        processtracker update --topic Actor --initial-name 'New Actor' --name 'Modified Actor'

Shorthand can also be used:::

        processtracker update -t Actor -i 'New Actor' -n 'Modified Actor'

Delete
------

This allows for existing lookup records to be deleted.::

        processtracker delete --topic Actor --name 'New Actor'

Shorthand can also be used:::

        processtracker delete -t Actor -n 'New Actor'
