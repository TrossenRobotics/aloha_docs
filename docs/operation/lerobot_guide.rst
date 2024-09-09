========================
LeRobot X Aloha User Guide
========================

Virtual Environment Setup
=========================

Containerization is crucial for running machine learning models to avoid dependency conflicts. You can either use a Virtual Environment (venv) or Conda for this purpose.

.. _virtual_env_setup:

Using Virtual Environment (venv)
--------------------------------

#. Install the virtual environment package:

   .. code-block:: bash

      $ sudo apt-get install python3-venv

#. Create a virtual environment:

   .. code-block:: bash

      $ python3 -m venv ~/lerobot  # Creates a venv "lerobot" in the home directory (you can create it anywhere)

#. Activate the virtual environment:

   .. code-block:: bash

      $ source ~/lerobot/bin/activate

Using Conda
-----------

#. Create a virtual environment:

   .. code-block:: bash

      $ conda create -n lerobot python=3.10

#. Activate the virtual environment:

   .. code-block:: bash

      $ conda activate lerobot

.. note:: Use either `venv` or `Conda` based on your preference, but do not mix them to avoid dependency issues.

Clone Repository
================

For users of Aloha Stationary:

#. Clone the LeRobot repository:

   .. code-block:: bash

      $ cd ~
      $ git clone https://github.com/huggingface/lerobot.git

Build and Install LeRobot Models
================================

#. Build and install the LeRobot models from source:

   .. code-block:: bash

      $ cd lerobot && pip install -e .

Teleoperation
=============

To teleoperate your robot, follow these steps:

#. Find the serial numbers of your robot's arms and cameras as described in the following documentation:
   
   - Arm Symlink Setup: https://docs.trossenrobotics.com/aloha_docs/getting_started/stationary/software_setup.html#arm-symlink-setup
   - Camera Setup: https://docs.trossenrobotics.com/aloha_docs/getting_started/stationary/software_setup.html#camera-setup

#. Update the serial numbers in the configuration file: :file:`lerobot/common/configs/robot/aloha.yaml`

#. Run the teleoperation script:

   .. code-block:: bash

      python lerobot/scripts/control_robot.py teleoperate \
        --robot-path lerobot/configs/robot/aloha.yaml

.. note:: You will see logs that include information such as delta time (dt), frequency, and read/write times for the robot arms.

#. You can control the teleoperation frequency using the `--fps` argument. For example, to set it to 30 FPS:

   .. code-block:: bash

      python lerobot/scripts/control_robot.py teleoperate \
        --robot-path lerobot/configs/robot/aloha.yaml --fps 30



Recording Data Episodes
=======================

The system supports episode-based data collection, where episodes are time-bounded sequences of robot actions.

#. You can control the recording flow with arguments:

   - `--warmup-time-s`: Number of seconds for device warmup (default: 10s)
   - `--episode-time-s`: Number of seconds per episode (default: 60s)
   - `--reset-time-s`: Time for resetting after each episode (default: 60s)
   - `--num-episodes`: Number of episodes to record (default: 50)

Example:

.. code-block:: bash

   python lerobot/scripts/control_robot.py record \
     --robot-path lerobot/configs/robot/aloha.yaml \
     --fps 30 \
     --root data \
     --repo-id ${HF_USER}/aloha_test \
     --tags tutorial \
     --warmup-time-s 5 \
     --episode-time-s 30 \
     --reset-time-s 30 \
     --num-episodes 2

#. To push your dataset to Hugging Face's Hub, log in with a write-access token:

.. code-block:: bash

   huggingface-cli login --token ${HUGGINGFACE_TOKEN} --add-to-git-credential

.. note:: Set your Hugging Face username as a variable for ease:

.. code-block:: bash

   HF_USER=$(huggingface-cli whoami | head -n 1)

Visualizing Datasets
====================

To visualize all the episodes recorded in your dataset, you can run:

.. code-block:: bash

   python lerobot/scripts/visualize_dataset_html.py \
     --root data \
     --repo-id ${HF_USER}/aloha_test

.. tip:: This will launch a local web server that uses `Rerun.io` to display camera streams and robot states in video format.

Replay Recorded Episodes
========================

Replaying episodes allows you to test the repeatability of the robot's actions. To replay the first episode of your recorded dataset:

.. code-block:: bash

   python lerobot/scripts/control_robot.py replay \
     --robot-path lerobot/configs/robot/aloha.yaml \
     --fps 30 \
     --root data \
     --repo-id ${HF_USER}/aloha_test \
     --episode 0

.. note:: The robot should replicate the movements recorded in the episode.

Upload Policy Checkpoints
=========================

After training, upload the latest policy checkpoints to Hugging Face:

.. code-block:: bash

   huggingface-cli upload ${HF_USER}/act_aloha_test \
     outputs/train/act_aloha_test/checkpoints/last/pretrained_model

To upload intermediate checkpoints:

.. code-block:: bash

   CKPT=010000
   huggingface-cli upload ${HF_USER}/act_aloha_test_${CKPT} \
     outputs/train/act_aloha_test/checkpoints/${CKPT}/pretrained_model

Troubleshooting
===============

If you encounter issues, follow these troubleshooting steps:

#. **OpenCV Installation Issues (Linux)**

   - Uninstall OpenCV using pip and reinstall using Conda:

     .. code-block:: bash

        pip uninstall opencv-python
        conda install -c conda-forge opencv=4.10.0

#. **FFmpeg Encoding Error: `unknown encoder libsvtav1`**

   - Install FFmpeg with `libsvtav1` support via Conda-Forge or Homebrew:

     .. code-block:: bash

        conda install -c conda-forge ffmpeg

     or:

     .. code-block:: bash

        brew install ffmpeg

#. **Arrow Keys Not Working During Data Recording (Linux)**

   - Ensure that the `$DISPLAY` environment variable is set correctly.

.. warning:: Avoid adding too much variation to data collection too quickly, as it can negatively impact training results.


