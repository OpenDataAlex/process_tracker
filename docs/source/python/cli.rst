Command Line Tool
#################

The command line tool is designed to help with adding lookup values as well as work with the data store backend.  Once
the processtracker package is installed, the following options will be available from the command line.

Data Store Setup
****************

When first setting up processtracker, the CLI can be leveraged to create all the data store objects:::

        processtracker setup

If the data store has already been created, but you want to start fresh, pass the overwrite option:::

        processtracker setup --overwrite True

There is also a shorthand version:::

        processtracker setup -o True

Lookup Objects
**************

The following lookup objects can be modified or created via the command line:

* Actor
* Error Type
* Extract Status
* Process Dependency
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

Process Dependency
------------------

Process dependencies can be added and removed via the CLI.  These are a bit more complicated to work with than the
lookup objects.

Create
^^^^^^

To create a new process dependency, use the create command as noted above, but with other parameters.::

        processtracker create --topic "process dependency" --parent "My Parent Process Name" --child "My Child Process Name"

Shorthand can also be used:::

        processtracker create -t "process dependency" -p "My Parent Process Name" -c "My Child Process Name"

Delete
^^^^^^

To delete a new process dependency, use the delete command as noted above, but with other parameters.::

        processtracker delete --topic "process dependency" --parent "My Parent Process Name" --child "My Child Process Name"

Shorthand can also be used:::

        processtracker delete -t "process dependency" -p "My Parent Process Name" -c "My Child Process Name"
