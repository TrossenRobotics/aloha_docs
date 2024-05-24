===============================
Stationary ALOHA Software Setup
===============================

.. note::

  The Interbotix ALOHA Stationary control software is supported for **ROS 2 Humble** running on **native Linux Ubuntu 22.04**.

.. note::

  If using a laptop supplied by Trossen Robotics, all required software will be pre-installed.
  In this case, you can skip to the Post-Install Hardware Setup section of this guide.

This guide will walk through the process of setting up the Interbotix ALOHA Stationary control software.

ROS 2 Installation
==================

ROS 2 can be installed in one of two ways:

* (**Preferred**) Using the Interbotix X-Series Arm installation script.
  See the next section for details.
* Following the `official ROS 2 Humble Ubuntu Debian install guide`_.

.. _`official ROS 2 Humble Ubuntu Debian install guide`: https://docs.ros.org/en/humble/Installation/Ubuntu-Install-Debians.html

Interbotix X-Series Arm Control Software Installation
=====================================================

Follow the `ROS 2 Standard Software Setup > AMD64 Architecture guide`_.

.. _`ROS 2 Standard Software Setup > AMD64 Architecture guide`: https://docs.trossenrobotics.com/interbotix_xsarms_docs/ros_interface/ros2/software_setup.html#amd64-architecture

In short, run the following commands on a machine running Linux Ubuntu 22.04:

  .. code-block:: bash

    $ sudo apt install curl
    $ curl 'https://raw.githubusercontent.com/Interbotix/interbotix_ros_manipulators/main/interbotix_ros_xsarms/install/amd64/xsarm_amd64_install.sh' > xsarm_amd64_install.sh
    $ chmod +x xsarm_amd64_install.sh
    $ ./xsarm_amd64_install.sh -d humble -n

The installation script does the following:

1.  Installs ROS 2 Humble (if not already installed).
2.  Creates the following directory structure:

  .. code-block::

    ~/interbotix_ws
      └── src
          ├── interbotix_ros_core
          ├── interbotix_ros_manipulators
          └── interbotix_ros_toolboxes

3.  Installs dependencies.
4.  Builds the control software and other related tools.
5.  Configures the ROS 2 environment.

.. tip::

  After running the install script, remove all added lines beneath the ``# Interbotix Configurations`` header in the ``~/.bashrc`` file.
  Doing so will make environment configuration easier later on.

ALOHA Software Installation
===========================

1.  Clone the Interbotix fork of ALOHA into the workspace's source directory:

  .. code-block:: bash

    $ cd ~/interbotix_ws/src
    $ git clone https://github.com/Interbotix/aloha.git

2.  Run rosdep to install any dependencies:

  .. code-block:: bash

    $ cd ~/interbotix_ws
    $ rosdep install --from-paths src --ignore-src -r -y

3.  Set the ``InterbotixManipulatorXS``'s ``iterative_update_fk`` default value to ``False`` at ``~/interbotix_ws/src/interbotix_ros_toolboxes/interbotix_xs_toolbox/interbotix_xs_modules/interbotix_xs_modules/xs_robot/arm.py`` (`link`_).

.. _`link`: https://github.com/Interbotix/interbotix_ros_toolboxes/blob/c187bcea89b60391244bb19943ebd78f770aa975/interbotix_xs_toolbox/interbotix_xs_modules/interbotix_xs_modules/xs_robot/arm.py#L81

4.  Add the following line to your ``~/.bashrc`` file:

  .. code-block:: bash

    $ echo "export INTERBOTIX_ALOHA_IS_MOBILE=false" >> ~/.bashrc

5.  Build the workspace:

  .. code-block:: bash

    $ cd ~/interbotix_ws
    $ colcon build

.. note::

  If planning to change the control or data collection software later on, you may want to do a symbolically-linked install.
  If that is the case, remove the build and install directories, and re-run ``colcon build`` with the ``--symlink-install`` flag.

    .. code-block:: bash

      $ cd ~/interbotix_ws
      $ rm -rf build install
      $ colcon build --symlink-install

Post-Install Hardware Setup
===========================

The arm and cameras need to be bound to a unique device.
The following sections will provide steps on setting up unique symbolic links for each device.

Arm Symlink Setup
-----------------

We will configure udev rules for the arms such that they are bound to the following device names:

* ``ttyDXL_leader_left``
* ``ttyDXL_leader_right``
* ``ttyDXL_follower_left``
* ``ttyDXL_follower_right``

To set these up, do the following:

1.  Plug in only the leader left robot to the computer.

2.  Determine its device name by checking the ``/dev`` directory before and after plugging the device in.
    This is likely something like ``/dev/ttyUSB0``.

3.  Print out the device serial number by running the following command:

  .. code-block:: bash

    $ udevadm info --name=/dev/ttyUSB0 --attribute-walk | grep ATTRS{serial} | head -n 1 | cut -d '"' -f2
    FT88YWBJ

4.  The output of the command will look like ``FT88YWBJ`` and be the serial number of the arm's U2D2 serial converter.

5.  Add the following line to the computer's fixed Interbotix udev rules at ``/etc/udev/rules.d/99-fixed-interbotix-udev.rules``:

  .. code-block:: bash

    SUBSYSTEM=="tty", ATTRS{serial}=="<SERIAL NUMBER>", ENV{ID_MM_DEVICE_IGNORE}="1", ATTR{device/latency_timer}="1", SYMLINK+="ttyDXL_leader_left"
    #                                 ^^^^^^^^^^^^^^^ The result from the previous step

6.  Repeat for the rest of the arms.

7.  To update and refresh the rules, run the following command:

  .. code-block:: bash

    $ sudo udevadm control --reload && sudo udevadm trigger

8.  Plug all arms back into the computer and verify that you can see all devices:

  .. code-block:: bash

    $ ls /dev | grep ttyDXL_
    ttyDXL_leader_left
    ttyDXL_leader_right
    ttyDXL_follower_left
    ttyDXL_follower_right

Camera Setup
------------

1.  Open realsense-viewer

  .. code-block::

    $ realsense-viewer

2.  Plug in a single camera and check the sidebar for its entry.
    If it does not show up in the side bar, click **Add Source** and find the Intel RealSense D405 in the drop down.

3.  Click on Info for the camera, find the Serial Number, and copy it.

  .. image:: images/rsviewer_serialno.png
    :align: center

4.  Put the camera serial number in the appropriate config entry at ``~/interbotix_ws/src/aloha/config/rs_cam.yaml``.

5.  Repeat for the rest of the cameras.
    If the workspace has not been symbolically-linked, a rebuild may be necessary.

Post-Install Software Tips
==========================

Disable wandb
-------------

It may be helpful to disable wandb while getting started.
To do so, run the command below.
Note that this line is added by default to the laptops distributed by Trossen Robotics.

.. code-block:: bash

  $ echo "WANDB_MODE=disabled" >> ~/.bashrc

Alias Setup
-----------

It may be helpful to create bash aliases to make environment configuration easier.

Create a ~/.bash_aliases file:

.. code-block:: bash

  $ touch ~/.bash_aliases

To create an alias that can be used to set up the ROS 2 environment, add the following line to the ``~/.bash_aliases`` file:

.. code-block:: bash

  alias setup_aloha="source /opt/ros/humble/setup.bash && source ~/interbotix_ws/install/setup.bash"

Assuming that dependencies of ACT were installed using a venv, to create an alias that can be used to set up the ROS 2 and ACT environments, add the following line to the ``~/.bash_aliases`` file:

.. code-block:: bash

  alias setup_act="setup_aloha && source ~/act/bin/activate"
