====================
Gravity Compensation
====================

During the teleoperation and data collection operations, the ``-g`` argument effectively creates a ros2 client that calls the `interbotix_gravity_compensation`_ node to enable/disable the gravity compensation feature.
For more details on its configuration and usage, please check out the `interbotix_xsarm_gravity_compensation`_ package, a toy example running gravity compensation on a single arm.

.. _`interbotix_gravity_compensation`: https://github.com/Interbotix/interbotix_ros_toolboxes/tree/humble/interbotix_common_toolbox/interbotix_gravity_compensation
.. _`interbotix_xsarm_gravity_compensation`: https://docs.trossenrobotics.com/interbotix_xsarms_docs/ros2_packages/gravity_compensation.html