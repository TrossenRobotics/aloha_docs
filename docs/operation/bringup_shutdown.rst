==================
Bringup & Shutdown
==================

Bringup
=======

To bring up a Mobile or Stationary ALOHA, you will need to run the following commands in a terminal:

.. code-block:: bash

  $ export INTERBOTIX_ALOHA_IS_MOBILE=true/false # true for Mobile, false for Stationary
  $ source /opt/ros/humble/setup.bash # configure ROS system install environment
  $ source ~/interbotix_ws/install/setup.bash # configure ROS workspace environment
  $ ros2 launch aloha aloha_bringup.launch.py # launch hardware drivers and control software

.. warning::

  Terminating bringup while the arms are not in their sleep configurations will cause them to collapse!
  Be sure to run through the :ref:`operation/bringup_shutdown:shutdown` procedure before doing so.

.. tip::

  The ALOHA can be brought up with additional arguments to customize the behavior.
  For a full list of arguments, refer to the :ref:`operation/bringup_shutdown:Arguments for ALOHA Bringup` section below.

Shutdown
========

To shutdown the ALOHA, you will need to first send the arms to their sleep configurations.
While the ALOHA bringup is running in another terminal, open a new one and run the following commands:

.. code-block:: bash

  $ export INTERBOTIX_ALOHA_IS_MOBILE=true/false # true for Mobile, false for Stationary
  $ source /opt/ros/humble/setup.bash # configure ROS system install environment
  $ source ~/interbotix_ws/install/setup.bash # configure ROS workspace environment
  $ cd ~/interbotix_ws/src/aloha/scripts/
  $ python3 sleep.py

The follower arms will move to their "staged" configurations and then place themselves into their sleep configurations.

.. tip::

  You can optionally append the ``-a|--all`` flag to the sleep script command to send all arms to their sleep configurations:

  .. code-block:: bash

    $ python3 sleep.py -a
    # or
    $ python3 sleep.py --all

Now that the arms are in their sleep configurations, you can terminate the ALOHA bringup by pressing :kbd:`Ctrl` + :kbd:`C` in the terminal where the bringup was launched.

What's Next?
============

Now that you know how to bringup and shutdown the ALOHA, teleoperation will be a good starting point to explore its features.

-   :doc:`/operation/mobile`
-   :doc:`/operation/stationary`

Arguments for ALOHA Bringup
===========================

.. csv-table::
  :file: ../_data/bringup.csv
  :header-rows: 1
  :widths: 20, 60, 20, 20
