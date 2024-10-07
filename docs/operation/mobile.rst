================
Mobile Operation
================

Teleoperation
=============

While ALOHA bringup is running in another terminal, open a new one and run the following commands:

.. code-block:: bash

  $ export INTERBOTIX_ALOHA_IS_MOBILE=true # if not already in your environment
  $ source /opt/ros/humble/setup.bash # configure ROS system install environment
  $ source ~/interbotix_ws/install/setup.bash # configure ROS workspace environment
  $ cd ~/interbotix_ws/src/aloha/scripts/
  $ python3 dual_side_teleop.py [-g]

The arms will lift themselves up into their "staged" configurations.
Close both grippers on the leader arms to begin teleop.

You should now be able to teleoperate both sets of arms.
When you finish, place the leader arms in their cradles.
Press :kbd:`Ctrl` + :kbd:`C` on the teleoperation terminal to stop teleoperation.

.. tip::

  If the ``-g`` argument is set, the :doc:`gravity compensation </operation/gravity_compensation>` feature (introduced since 10/04/2024) will be enabled for the leader robots when teleop starts.
  Otherwise, the leader arms will be torqued off.

.. warning::

  Even with the ``-g`` argument set, the arms **WILL** still torque off and drop for a short period of time while enabling/disabling the :doc:`gravity compensation </operation/gravity_compensation>` feature.

  Please make sure they are readily held while closing the grippers and placed in the cradles before sending them to the sleep configuration.

.. tip::

  While doing teleop, another person can use the included PS4 controller to move the base.

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

What's Next?
============

Now that you know how to teleoperate, you can start :doc:`collecting data </operation/data_collection>` with your ALOHA.
