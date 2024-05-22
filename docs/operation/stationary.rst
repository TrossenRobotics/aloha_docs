====================
Stationary Operation
====================

Running ALOHA Bringup
=====================

In a terminal, run the following commands:

.. code-block:: bash

  $ export INTERBOTIX_ALOHA_IS_MOBILE=false # if not already in your environment
  $ source /opt/ros/humble/setup.bash # configure ROS system install environment
  $ source ~/interbotix_ws/install/setup.bash # configure ROS workspace environment
  $ ros2 launch aloha aloha_bringup.launch.py # launch hardware drivers and control software

.. warning::

  Terminating bringup while the follower arms are not in their sleep configurations will cause them to collapse!
  Be sure to run the :ref:`operation/stationary:Sending Arms to Sleep Configuration` process before doing so.

Teleoperation
=============

While ALOHA bringup is running in another terminal, open a new one and run the following commands:

.. code-block:: bash

  $ export INTERBOTIX_ALOHA_IS_MOBILE=false # if not already in your environment
  $ source /opt/ros/humble/setup.bash # configure ROS system install environment
  $ source ~/interbotix_ws/install/setup.bash # configure ROS workspace environment
  $ source ~/aloha/bin/activate # configure ALOHA Python environment
  $ cd ~/interbotix_ws/src/aloha/scripts/
  $ python3 dual_side_teleop.py

The arms will lift themselves up into their "staged" configurations.
Close both grippers on the leader arms to begin teleop.

You should now be able to teleoperate both sets of arms.
Press :kbd:`Ctrl` + :kbd:`C` on the teleoperation terminal to stop teleoperation.
Place the leader arms in their cradles.

Sending Arms to Sleep Configuration
===================================

While ALOHA bringup is running in another terminal, open a new one and run the following commands:

.. code-block:: bash

  $ export INTERBOTIX_ALOHA_IS_MOBILE=false # if not already in your environment
  $ source /opt/ros/humble/setup.bash # configure ROS system install environment
  $ source ~/interbotix_ws/install/setup.bash # configure ROS workspace environment
  $ source ~/aloha/bin/activate # configure ALOHA Python environment
  $ cd ~/interbotix_ws/src/aloha/scripts/
  $ python3 sleep.py

The follower arms will move to their "staged" configurations and then place themselves into their sleep configurations.

.. tip::

  You can optionally append the ``-a|--all`` flag to the sleep script command to send all arms to their sleep configurations:

  .. code-block:: bash

    $ python3 sleep.py -a
    # or
    $ python3 sleep.py --all

.. Episode Collection
.. ==================



.. Automatic Episode Collection
.. ============================
