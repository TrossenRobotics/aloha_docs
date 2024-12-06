==================
Bringup & Shutdown
==================

Bringup
=======

To bring up an ALOHA robot configuration (Mobile, Solo, or Stationary), you need to specify the robot type and run the following commands in a terminal:

.. code-block:: bash

  $ source /opt/ros/humble/setup.bash # configure ROS system install environment
  $ source ~/interbotix_ws/install/setup.bash # configure ROS workspace environment
  $ ros2 launch aloha aloha_bringup.launch.py robot:=<configuration> # launch hardware drivers and control software

.. important::

  The robot argument is required to specify which ALOHA configuration to bring up.
  Choose the value that matches your setup, such as ``aloha_static``, ``aloha_solo``, or ``aloha_mobile``.

.. warning::

  Terminating bringup while the arms are not in their sleep configurations will cause them to collapse!
  Be sure to run through the :ref:`operation/bringup_shutdown:shutdown` procedure before doing so.

.. tip::

  The ALOHA can be brought up with different configurations and additional arguments to customize the behavior.
  Please refer to the :ref:`operation/bringup_shutdown:Configurations` section below for details.

Shutdown
========

To shutdown the ALOHA, you will need to first send the arms to their sleep configurations.
While the ALOHA bringup is running in another terminal, open a new one and run the following commands:

.. code-block:: bash

  $ export INTERBOTIX_ALOHA_IS_MOBILE=true/false # true for Mobile, false for Stationary
  $ source /opt/ros/humble/setup.bash # configure ROS system install environment
  $ source ~/interbotix_ws/install/setup.bash # configure ROS workspace environment
  $ cd ~/interbotix_ws/src/aloha/scripts/
  $ python3 sleep.py -r <configuration>

.. important::

  The robot argument is required to specify which ALOHA configuration to bring up.
  Choose the value that matches your setup, such as ``aloha_static``, ``aloha_solo``, or ``aloha_mobile``.

The follower arms will move to their "staged" configurations and then place themselves into their sleep configurations.

.. tip::

  You can optionally append the ``-a|--all`` flag to the sleep script command to send all arms to their sleep configurations:

  .. code-block:: bash

    $ python3 sleep.py -r <configuration> -a
    # or
    $ python3 sleep.py -r <configuration> --all

Now that the arms are in their sleep configurations, you can terminate the ALOHA bringup by pressing :kbd:`Ctrl` + :kbd:`C` in the terminal where the bringup was launched.

What's Next?
============

Now that you know how to bringup and shutdown the ALOHA, teleoperation will be a good starting point to explore its features.

-   :doc:`/operation/mobile`
-   :doc:`/operation/stationary`
-   :doc:`/operation/solo`

Configurations
==============

The configuration yaml files provided in the ``~/interbotix_ws/src/aloha/config`` directory can be used to customize the behavior of the ALOHA.
Please follow the links below to see the details of each configuration file:

-   SLATE Robot Base (only for Mobile ALOHA)

    -   `teleop_twist_joy Parameters`_:

        -   ``base_joystick_teleop.yaml``

-   Interbotix Arms

    -   `Mode Configs`_:

        -   ``leader_modes_left.yaml``
        -   ``leader_modes_right.yaml``
        -   ``follower_modes_left.yaml``
        -   ``follower_modes_right.yaml``

    -   `Motor Specs`_:

        -   ``leader_motor_specs_left.yaml``
        -   ``leader_motor_specs_right.yaml``


.. _`teleop_twist_joy Parameters`: https://docs.ros.org/en/humble/p/teleop_twist_joy/index.html#parameters
.. _`Mode Configs`: https://docs.trossenrobotics.com/interbotix_xsarms_docs/ros_interface/ros2/config.html#mode-configs
.. _`Motor Specs`: https://docs.trossenrobotics.com/interbotix_xsarms_docs/ros2_packages/gravity_compensation.html#configuration
.. _`realsense2_camera Parameters`: https://github.com/IntelRealSense/realsense-ros/tree/ros2-development?tab=readme-ov-file#parameters

Besides the default configuration files, the launch file ``aloha_bringup.launch.py`` provides additional arguments for further customization.
Please refer to the following table for details:

.. csv-table::
  :file: ../_data/bringup.csv
  :header-rows: 1
  :widths: 20, 60, 20, 20
