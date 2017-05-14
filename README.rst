============
stacktask-ui
============

StackTask Dashboard

* Free software: Apache license
* Source: https://github.com/catalyst/stacktask-ui

Manual Installation
-------------------

Begin by cloning the Horizon and StackTask UI repositories::

    git clone https://github.com/openstack/horizon
    git clone https://github.com/catalyst/stacktask-ui

Create a virtual environment and install Horizon dependencies::

    cd horizon
    python tools/install_venv.py

Set up your ``local_settings.py`` file::

    cp openstack_dashboard/local/local_settings.py.example openstack_dashboard/local/local_settings.py

Open up the copied ``local_settings.py`` file in your preferred text
editor. You will want to customize several settings:

-  ``OPENSTACK_HOST`` should be configured with the hostname of your
   OpenStack server. Verify that the ``OPENSTACK_KEYSTONE_URL`` and
   ``OPENSTACK_KEYSTONE_DEFAULT_ROLE`` settings are correct for your
   environment. (They should be correct unless you modified your
   OpenStack server to change them.)
-  ``OPENSTACK_REGISTRATION_URL`` should also be configured to point to
   you StackTask server and version.

You will also need to update the ``keystone_policy.json`` file (openstack_dashboard/conf/keystone_policy.json) in horizon with
the following lines::

    "project_mod": "role:project_mod",
    "project_admin": "role:project_admin",
    "project_mod_or_admin": "rule:admin_required or rule:project_mod or rule:project_admin",
    "project_admin_only": "rule:admin_required or rule:project_admin",
    "identity:project_users_access": "rule:project_mod_or_admin",

Install StackTask UI with all dependencies in your virtual environment::

    tools/with_venv.sh pip install -e ../stacktask-ui/

And enable it in Horizon::

    cp ../stacktask-ui/stacktask-ui/enabled/_* openstack_dashboard/local/enabled

To run horizon with the newly enabled StackTask UI plugin run::

    ./run_tests.sh --runserver 0.0.0.0:8080

to have the application start on port 8080 and the horizon dashboard will be
available in your browser at http://localhost:8080/
