====================
Stationary Operation
====================

Teleoperation
=============

While ALOHA bringup is running in another terminal, open a new one and run the following commands:

.. code-block:: bash

  $ export INTERBOTIX_ALOHA_IS_MOBILE=false # if not already in your environment
  $ source /opt/ros/humble/setup.bash # configure ROS system install environment
  $ source ~/interbotix_ws/install/setup.bash # configure ROS workspace environment
  $ source /<path_to_aloha_venv>/bin/activate # configure ALOHA Python environment
  $ cd ~/interbotix_ws/src/aloha/scripts/
  $ python3 dual_side_teleop.py [-g]

The arms will lift themselves up into their "staged" configurations.
Close both grippers on the leader arms to begin teleop.

.. tip::

  If the mechanical gravity compensation system is not preferred for your application, we also have a software solution introduced since 10/04/2024.

  You can detach the pulley system from the leader arms and set the ``-g`` argument.
  This will enable the gravity compensation feature for the leader robots when teleop starts.

.. warning::

  With the ``-g`` argument set, the arms **WILL** still torque off and drop for a short period of time while enabling/disabling the gravity compensation feature.
  Please make sure they are readily held while closing the grippers and placed in the cradles before sending them to the sleep configurations.

You should now be able to teleoperate both sets of arms.
When you finish, place the leader arms in their cradles.
Press :kbd:`Ctrl` + :kbd:`C` on the teleoperation terminal to stop teleoperation.

What's Next?
============

Now that you know how to teleoperate, you can start :doc:`collecting data </operation/data_collection>` with your ALOHA.
