====================
Solo Operation
====================

Teleoperation
=============

While ALOHA bringup is running in another terminal, open a new one and run the following commands:

.. code-block:: bash

  $ source /opt/ros/humble/setup.bash # configure ROS system install environment
  $ source ~/interbotix_ws/install/setup.bash # configure ROS workspace environment
  $ cd ~/interbotix_ws/src/aloha/scripts/
  $ python3 teleop.py -r aloha_solo [-g]

The arms will lift themselves up into their "staged" configurations.
Close both grippers on the leader arms to begin teleop.

You should now be able to teleoperate both sets of arms.
When you finish, place the leader arms in their cradles.
Press :kbd:`Ctrl` + :kbd:`C` on the teleoperation terminal to stop teleoperation.

.. tip::

  The Aloha Solo does not include a mechanical gravity compensation system.
  Instead, it is recommended to use the software-based gravity compensation feature, as it is more convenient for recording episodes over extended periods.
  However, for smoother and more precise control of the arms, you may choose to disable gravity compensation.
  Be aware that this can put additional strain on the user's arms during operation.
  You can set the ``-g`` argument.
  This will enable the :doc:`gravity compensation </operation/gravity_compensation>` feature for the leader robots when teleop starts.

.. warning::

  With the ``-g`` argument set, the arms **WILL** still torque off and drop for a short period of time while enabling/disabling the :doc:`gravity compensation </operation/gravity_compensation>` feature.
  Please make sure they are readily held while closing the grippers and placed in the cradles before sending them to the sleep configurations.

What's Next?
============

Now that you know how to teleoperate, you can start :doc:`collecting data </operation/data_collection>` with your ALOHA.
