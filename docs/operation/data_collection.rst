===============
Data Collection
===============

Task Creation
=============

Task configuration can be found in the ALOHA package's aloha module under ``constants.py`` in the ``TASK_CONFIGS`` dictionary.
A template task (``aloha_template``) is provided containing all possible fields and some placeholder values.
Here, we will focus only on the task name, the dataset directory, the episode length, and the observation cameras.

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
  * - Camera Names
    - The ``camera_names`` field takes in a list of strings corresponding to camera names.
      These camera names will be used as observation sources during dataset collection.

Recording Episodes
==================

To record an episode, follow the steps below:

#.  Bring up the ALOHA control stack according to your platform

    * Stationary: :ref:`operation/stationary:Running ALOHA Bringup`
    * Mobile: :ref:`operation/mobile:Running ALOHA Bringup`

#.  Configure the environment and run the episode recording script as follows:

  .. code-block:: bash

    $ source /opt/ros/humble/setup.bash # configure ROS system install environment
    $ source ~/interbotix_ws/install/setup.bash # configure ROS workspace environment
    $ source /<path_to_aloha_venv>/bin/activate # configure ALOHA Python environment
    $ cd ~/interbotix_ws/src/aloha/scripts/
    $ python3 record_episodes.py --task_name <task_name> --episode_idx <episode_idx>

  .. note::

    The ``task_name`` argument should match one of the task names in the ``TASK_CONFIGS``, as configured in the :ref:`operation/data_collection:Task Creation` section.

  .. note::

    Each platform has a "dummy" task that can be used to test basic data collection and playback.
    For the Stationary variant, use the ``aloha_stationary_dummy`` task.
    For the Mobile variant, use the ``aloha_mobile_dummy`` task.

    An example for the Mobile variant would look like:

    .. code-block:: bash

      $ python3 record_episodes.py --task_name aloha_mobile_dummy --episode_idx 0

Episode Playback
================

To play back a previously-recorded episode, follow the steps below:

#.  Bring up the ALOHA control stack according to your platform

    * Stationary: :ref:`operation/stationary:Running ALOHA Bringup`
    * Mobile: :ref:`operation/mobile:Running ALOHA Bringup`

#.  Configure the environment and run the episode playback script as follows:

  .. code-block:: bash

    $ source /opt/ros/humble/setup.bash # configure ROS system install environment
    $ source ~/interbotix_ws/install/setup.bash # configure ROS workspace environment
    $ source /<path_to_aloha_venv>/bin/activate # configure ALOHA Python environment
    $ cd ~/interbotix_ws/src/aloha/scripts/
    $ python3 replay_episodes.py --dataset_dir </path/to/dataset> --episode_idx <episode_idx>

  .. note::

    An example for replaying the dummy Mobile episode recorded above would look like:

    .. code-block:: bash

      $ python3 replay_episodes.py --dataset_dir ~/aloha_data/aloha_mobile_dummy/ --episode_idx 0
