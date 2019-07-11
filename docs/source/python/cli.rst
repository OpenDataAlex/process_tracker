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
* Cluster
* Cluster Process
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

Cluster Process
---------------

Cluster Process relationships can be added and removed via the CLI.  These are a bit more complicated to work with than the
lookup objects.

Create
^^^^^^

To create a new cluster process relationship, use the create command as noted above, but with other parameters.::

        processtracker create --topic "cluster process" --cluster "My Cluster" --child "My Process Name"

Shorthand can also be used.::

        processtracker create -t "cluster process" --cluster "My Cluster" -c "My Process Name"

Delete
^^^^^^

To delete a cluster process relationship, use the delete command as noted above, but with other parameters.::

        processtracker delete --topic "cluster process" --cluster "My Cluster" --child "My Process Name"

Shorthand can also be used.::

        processtracker delete -t "cluster process" --cluster "My Cluster" -c "My Process Name"


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

To delete a process dependency, use the delete command as noted above, but with other parameters.::

        processtracker delete --topic "process dependency" --parent "My Parent Process Name" --child "My Child Process Name"

Shorthand can also be used:::

        processtracker delete -t "process dependency" -p "My Parent Process Name" -c "My Child Process Name"

.. _password_encryption:

Password Encryption
*******************

Data store passwords can be encrypted using the CLI tool.  This encryption is NOT cryptographically secure!  This method
is just so plain text passwords are not stored in the configuration file.::

        processtracker encrypt --password "MySecretPassword"

This will return back the encrypted cypher:::

        Encrypted password is now:  Encrypted wqfCvsKKwpzCrsOYw4rCvsKBwrHCrMOawr_DlcOZwro=
        When storing in your config file, please be sure to include 'Encrypted ' as well as the hash.

As the message states, take the password, starting with the 'Encrypted ' and paste it into your config file.  Again, this
is not cryptographically secure.  This only helps obscure your password.