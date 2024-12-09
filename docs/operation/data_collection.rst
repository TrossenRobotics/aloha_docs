===============
Data Collection
===============

Task Creation
=============

Task configurations are set up in the ``tasks_config.yaml`` file located inside the ``config`` folder in the Aloha package.
A template task (aloha_template) is provided within the file, which includes all possible fields and placeholder values.
For this guide, we will focus on configuring the task name, dataset directory, and episode length.

.. list-table::
  :align: center
  :widths: 25 75
  :header-rows: 1
  :width: 60%

  * - Configuration Field
    - Description
  * - Task Name
    - The task name should accurately describe the task that the ALOHA is performing.
  * - Dataset Directory
    - The ``dataset_dir`` field sets the directory episodes will saved to.
  * - Episode Length
    - The ``episode_len`` field sets the length of the episode in number of timesteps.


Recording Episodes
==================

To record an episode, follow the steps below:

#.  :ref:`operation/bringup_shutdown:bringup` the ALOHA control stack according to your platform

#.  Configure the environment and run the episode recording script as follows:

  .. code-block:: bash

    $ source /opt/ros/humble/setup.bash # configure ROS system install environment
    $ source ~/interbotix_ws/install/setup.bash # configure ROS workspace environment
    $ cd ~/interbotix_ws/src/aloha/scripts/
    $ python3 record_episodes.py \
          --task_name <task_name> \
          --robot <robot_configuration> \
          [--episode_idx <episode_idx>] \
          [-b, --enable_base_torque] \
          [-g, --gravity_compensation]

  The ``task_name`` argument should match one of the task names defined in the ``tasks_config.yaml`` file, as configured in the :ref:`operation/data_collection:Task Creation` section.

  The ``-r, --robot`` argument should correspond to one of the configurations present in the ``config/robot`` directory.
  Supported robot names include ``aloha_solo``, ``aloha_mobile``, and ``aloha_stationary``.

  The ``episode_idx`` argument is optional and specifies the index of the episode to record reflected in the output filename ``<dataset_dir>/episode_<episode_idx>.hdf5``.
  If not provided, the script will automatically calculate the next episode index based on the number of episodes already saved in the dataset directory.

  If the ``-b, --enable_base_torque`` argument is set, mobile base will be torqued on during episode recording, allowing the use of a joystick controller or some other manual method.

  If the ``-g, --gravity_compensation`` argument is set, gravity compensation will be enabled for the leader robots when teleop starts.

  .. tip::

    Each platform has a "dummy" task that can be used to test basic data collection and playback.
    For the Stationary variant, use the ``aloha_stationary_dummy`` task.
    For the Mobile variant, use the ``aloha_mobile_dummy`` task.
    For the Solo variant, use the ``aloha_solo_dummy`` task.

    An example for the Mobile variant would look like:

    .. code-block:: bash

      $ python3 record_episodes.py --task_name aloha_mobile_dummy --robot aloha_mobile --episode_idx 0

Episode Playback
================

To play back a previously-recorded episode, follow the steps below:

#.  :ref:`operation/bringup_shutdown:bringup` the ALOHA control stack according to your platform

#.  Configure the environment and run the episode playback script as follows:

  .. code-block:: bash

    $ source /opt/ros/humble/setup.bash # configure ROS system install environment
    $ source ~/interbotix_ws/install/setup.bash # configure ROS workspace environment
    $ cd ~/interbotix_ws/src/aloha/scripts/
    $ python3 replay_episodes.py --robot <robot_configuration> --dataset_dir </path/to/dataset> --episode_idx <episode_idx>

  .. tip::

    An example for replaying the dummy Mobile episode recorded above would look like:

    .. code-block:: bash

      $ python3 replay_episodes.py --robot aloha_mobile --dataset_dir ~/aloha_data/aloha_mobile_dummy/ --episode_idx 0

Episode Auto-Recording
======================

A helpful bash script, ``auto_record.sh``, is provided to allow users to collect many episodes consecutively without having to interact with the control computer.

Configuration
-------------

This script, whose `source`_ can be found on the ALOHA GitHub repository, has a few configuration options that should be verified or set before running.

.. _`source`: https://github.com/Interbotix/aloha/blob/main/scripts/auto_record.sh

``ROS_DISTRO``
^^^^^^^^^^^^^^

Set the codename of the ROS distribution used on the control computer.
This value is used to set the path to the ``ROS_SETUP_PATH`` variable used later in the script.
``ROS_DISTRO`` defaults to ``humble``.

.. code-block:: bash

  ROS_DISTRO=humble

``ROS_SETUP_PATH``
^^^^^^^^^^^^^^^^^^

Set the path to the ROS distribution's setup script.
This value is used when setting up the system-installed ROS environment.
Setting the ``ROS_DISTRO`` variable from before should be sufficient to configure this variable.
``ROS_SETUP_PATH`` defaults to ``"/opt/ros/$ROS_DISTRO/setup.bash"``.

.. code-block:: bash

  ROS_SETUP_PATH="/opt/ros/$ROS_DISTRO/setup.bash"

``WORKSPACE_SETUP_PATH``
^^^^^^^^^^^^^^^^^^^^^^^^

Set the path to the Interbotix workspace's setup script.
This value is used when setting up the Interbotix workspace's ROS environment.
``WORKSPACE_SETUP_PATH`` defaults to ``"$HOME/interbotix_ws/install/setup.bash"``.

.. code-block:: bash

  WORKSPACE_SETUP_PATH="$HOME/interbotix_ws/install/setup.bash"

``RECORD_EPISODES``
^^^^^^^^^^^^^^^^^^^

Set the path to the ``record_episodes.py`` script.
This value is used when calling the record_episodes script.
``RECORD_EPISODES`` defaults to ``"$HOME/interbotix_ws/src/aloha/scripts/record_episodes.py"``.

.. code-block:: bash

  RECORD_EPISODES="$HOME/interbotix_ws/src/aloha/scripts/record_episodes.py"

Usage
-----

Once configured, the auto_record script is now ready to use. To auto-record a specific amount of episodes, follow the steps below:

#.  :ref:`operation/bringup_shutdown:bringup` the ALOHA control stack according to your platform

#.  In a new terminal, navigate to the directory storing the auto_record script and run the command below:

    .. code-block::

      $ auto_record.sh <task_name> <num_episodes> <robot_name> [-b, --enable_base_torque] [-g, --gravity_compensation]

    .. tip::

      An example for auto-recording 50 episodes of the dummy Mobile ALOHA task would look like:

      .. code-block:: bash

        $ auto_record.sh aloha_mobile_dummy 50 aloha_mobile

    The auto_record script will then call the record_episodes Python script the specified number of times.

    .. note::

      If episodes of the specified task already exist, episode indices will be automatically calculated as one greater than the number of tasks in the episode save directory.

Dataset Format
==============

ALOHA saves its episodes in the `hdf5 format`_ with the following format:

.. _`hdf5 format`: https://en.wikipedia.org/wiki/Hierarchical_Data_Format#HDF5


Aloha Stationary
----------------

.. code-block::

    - images
        - cam_high          (480, 640, 3) 'uint8'
        - cam_low           (480, 640, 3) 'uint8'
        - cam_left_wrist    (480, 640, 3) 'uint8'
        - cam_right_wrist   (480, 640, 3) 'uint8'
    - qpos                  (14,)         'float64'
    - qvel                  (14,)         'float64'

    action                  (14,)         'float64'

Aloha Mobile
------------

.. code-block::

    - images
        - cam_high          (480, 640, 3) 'uint8'
        - cam_left_wrist    (480, 640, 3) 'uint8'
        - cam_right_wrist   (480, 640, 3) 'uint8'
    - qpos                  (14,)         'float64'
    - qvel                  (14,)         'float64'

    action                  (14,)         'float64'
    base_action             (2,)          'float64'

Aloha Solo
----------

.. code-block::

    - images
        - cam_high          (480, 640, 3) 'uint8'
        - cam_left_wrist    (480, 640, 3) 'uint8'   or
        - cam_right_wrist   (480, 640, 3) 'uint8'
    - qpos                  (7,)          'float64'
    - qvel                  (7,)          'float64'

    action                  (7,)          'float64'


What's Next?
============

With the data collected, we are ready to :doc:`train and evaluate <../training>` the machine learning models.
