================
Mobile Operation
================

Running ALOHA Bringup
=====================

In a terminal, run the following commands:

.. code-block:: bash

  $ export INTERBOTIX_ALOHA_IS_MOBILE=true # if not already in your environment
  $ source /opt/ros/humble/setup.bash # configure ROS system install environment
  $ source ~/interbotix_ws/install/setup.bash # configure ROS workspace environment
  $ ros2 launch aloha aloha_bringup.launch.py # launch hardware drivers and control software

.. warning::

  Terminating bringup while the follower arms are not in their sleep configurations will cause them to collapse!
  Be sure to run the :ref:`operation/mobile:Sending Arms to Sleep Configuration` process before doing so.

Teleoperation
=============

While ALOHA bringup is running in another terminal, open a new one and run the following commands:

.. code-block:: bash

  $ export INTERBOTIX_ALOHA_IS_MOBILE=true # if not already in your environment
  $ source /opt/ros/humble/setup.bash # configure ROS system install environment
  $ source ~/interbotix_ws/install/setup.bash # configure ROS workspace environment
  $ source /<path_to_aloha_venv>/bin/activate # configure ALOHA Python environment
  $ cd ~/interbotix_ws/src/aloha/scripts/
  $ python3 dual_side_teleop.py

The arms will lift themselves up into their "staged" configurations.
Close both grippers on the leader arms to begin teleop.
While doing teleop, another person can use the included PS4 controller to move the base.

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
  $ source /<path_to_aloha_venv>/bin/activate # configure ALOHA Python environment
  $ cd ~/interbotix_ws/src/aloha/scripts/
  $ python3 sleep.py

The follower arms will move to their "staged" configurations and then place themselves into their sleep configurations.

.. tip::

  You can optionally append the ``-a|--all`` flag to the sleep script command to send all arms to their sleep configurations:

  .. code-block:: bash

    $ python3 sleep.py -a
    # or
    $ python3 sleep.py --all

Joystick Base Teleoperation
===========================

To teleoperate the base using the provided PS4 Bluetooth controller, you must first pair the controller with the control computer following the Mobile :ref:`getting_started/mobile/pairing_controller:Pairing Your Controller` guide.
Then proceed with the following steps:

#.  Turn on the PS4 controller by pressing the controller's :kbd:`PS` button.
#.  Bring up ALOHA, making sure that the ``use_base`` and ``use_joystick_teleop`` launch arguments are both set to ``true``.
    This is the default if your control computer environment is configured as Mobile.
#.  Press and hold the controller's :kbd:`L2` button to enable joystick control.
#.  Use the controller's left stick to send forwards or backwards linear velocity commands.
#.  Use the controller's right stick to send clockwise or counter-clockwise angular velocity commands.
#.  At any time, release the controllers :kbd:`L2` button to disable joystick control.
