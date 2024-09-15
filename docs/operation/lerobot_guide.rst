==========================
LeRobot X Aloha User Guide
==========================

Virtual Environment Setup
=========================

Containerization is crucial for running machine learning models to avoid dependency conflicts. You can either use a Virtual Environment (venv) or Conda for this purpose.

Using Virtual Environment (venv)
--------------------------------

1. Install the virtual environment package:

   .. code-block:: bash

      sudo apt-get install python3-venv

2. Create a virtual environment:

   .. code-block:: bash

      python3 -m venv ~/lerobot  # Creates a venv "lerobot" in the home directory

3. Activate the virtual environment:

   .. code-block:: bash

      source ~/lerobot/bin/activate

Using Conda
-----------

1. Create a virtual environment:

   .. code-block:: bash

      conda create -n lerobot python=3.10

2. Activate the virtual environment:

   .. code-block:: bash

      conda activate lerobot

.. note::
   Use either `venv` or `Conda` based on your preference, but **do not** mix them to avoid dependency issues.

Clone Repository
================

For users of Aloha Stationary:

1. Clone the LeRobot repository:

   .. code-block:: bash

      cd ~
      git clone https://github.com/huggingface/lerobot.git

Build and Install LeRobot Models
================================

1. Build and install the LeRobot models from source:

   .. code-block:: bash

      cd lerobot && pip install -e .

Teleoperation
=============

To teleoperate your robot, follow these steps:

1. Find the serial numbers of your robot's arms and cameras as described in the following documentation:
   
   - Arm Symlink Setup: `Aloha Post Install Arm Setup <https://docs.trossenrobotics.com/aloha_docs/getting_started/stationary/software_setup.html#arm-symlink-setup>`_
   - Camera Setup: `Aloha Post Install Camera Setup <https://docs.trossenrobotics.com/aloha_docs/getting_started/stationary/software_setup.html#camera-setup>`_

2. Update the serial numbers in the configuration file: :file:`lerobot/common/configs/robot/aloha.yaml`

3. Run the teleoperation script:

   .. code-block:: bash

      python lerobot/scripts/control_robot.py teleoperate \
         --robot-path lerobot/configs/robot/aloha.yaml

   You will see logs that include information such as delta time (dt), frequency, and read/write times for the robot arms.

4. You can control the teleoperation frequency using the `--fps` argument. For example, to set it to 30 FPS:

   .. code-block:: bash

      python lerobot/scripts/control_robot.py teleoperate \
         --robot-path lerobot/configs/robot/aloha.yaml --fps 30

Customizing Teleoperation with Hydra
-------------------------------------

You can override the default YAML configurations dynamically using Hydra syntax. For example, to change the USB ports of the leader and follower arms:

.. code-block:: bash

   python lerobot/scripts/control_robot.py teleoperate \
      --robot-path lerobot/configs/robot/aloha.yaml \
      --robot-overrides \
         leader_arms.main.port=/dev/tty.usbmodem575E0031751 \
         follower_arms.main.port=/dev/tty.usbmodem575E0032081

.. tip::
   If you don't have any cameras connected, you can exclude them using Hydra's syntax:

   .. code-block:: bash

      python lerobot/scripts/control_robot.py teleoperate \
         --robot-path lerobot/configs/robot/aloha.yaml \
         --robot-overrides '~cameras'

Recording Data Episodes
=======================

The system supports episode-based data collection, where episodes are time-bounded sequences of robot actions.

1. Control the recording flow with these arguments:

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

.. note::
   To push your dataset to Hugging Face's Hub, log in with a write-access token:

   .. code-block:: bash

      huggingface-cli login --token ${HUGGINGFACE_TOKEN} --add-to-git-credential

2. Set your Hugging Face username as a variable for ease:

   .. code-block:: bash

      HF_USER=$(huggingface-cli whoami | head -n 1)

Visualizing Datasets
====================

To visualize all the episodes recorded in your dataset, run:

.. code-block:: bash

   python lerobot/scripts/visualize_dataset_html.py \
      --root data \
      --repo-id ${HF_USER}/aloha_test

To visualize a single dataset episode from the Hugging Face Hub:

.. code-block:: bash

   python lerobot/scripts/visualize_dataset.py \
      --repo-id TrossenRoboticsCommunity/aloha_static_block_pickup \
      --episode-index 0

To visualize a single dataset episode stored locally:

.. code-block:: bash

   DATA_DIR='./my_local_data_dir' python lerobot/scripts/visualize_dataset.py \
      --repo-id TrossenRoboticsCommunity/aloha_static_block_pickup \
      --episode-index 0

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

.. tip::
   Use different `--fps` values to adjust the frequency of the robot actions.

Troubleshooting
===============

.. warning::
   If you encounter issues, follow these troubleshooting steps:

1. **OpenCV Installation Issues (Linux)**

   If you encounter OpenCV installation issues, uninstall it via `pip` and reinstall using Conda:

   .. code-block:: bash

      pip uninstall opencv-python
      conda install -c conda-forge opencv=4.10.0

2. **FFmpeg Encoding Error (`unknown encoder libsvtav1`)**

   Install FFmpeg with `libsvtav1` support via Conda-Forge or Homebrew:

   .. code-block:: bash

      conda install -c conda-forge ffmpeg

   Or:

   .. code-block:: bash

      brew install ffmpeg

3. **Arrow Keys Not Working During Data Recording (Linux)**

   Ensure that the `$DISPLAY` environment variable is set correctly.

Training 
========

To train a policy for controlling your robot, use the following command:

.. code-block:: bash

   DATA_DIR=data python lerobot/scripts/train.py \
      dataset_repo_id=${HF_USER}/aloha_test \
      policy=act_aloha_real \
      env=aloha_real \
      hydra.run.dir=outputs/train/act_aloha_test \
      hydra.job.name=act_aloha_test \
      device=cuda \
      wandb.enable=true

.. note::
   The arguments are explained below:

   1. We provided the dataset with `dataset_repo_id=${HF_USER}/aloha_test`.
   2. The policy is specified with `policy=act_aloha_real`. This configuration is loaded from `lerobot/configs/policy/act_aloha_real.yaml`.
   3. The environment is set with `env=aloha_real`. This configuration is loaded from `lerobot/configs/env/aloha_real.yaml`.
   4. The device is set to `cuda` to utilize an NVIDIA GPU for training.
   5. `wandb.enable=true` is used for visualizing training plots via [Weights and Biases](https://docs.wandb.ai/quickstart). Ensure you are logged in by running `wandb login`.

Upload Policy Checkpoints
=========================

Once training is complete, upload the latest checkpoint with:

.. code-block:: bash

   huggingface-cli upload ${HF_USER}/act_aloha_test \
      outputs/train/act_aloha_test/checkpoints/last/pretrained_model

To upload intermediate checkpoints:

.. code-block:: bash

   CKPT=010000
   huggingface-cli upload ${HF_USER}/act_aloha_test_${CKPT} \
      outputs/train/act_aloha_test/checkpoints/${CKPT}/pretrained_model

Evaluation
==========

To control your robot with the trained policy and record evaluation episodes:

.. code-block:: bash

   python lerobot/scripts/control_robot.py record \
      --robot-path lerobot/configs/robot/aloha.yaml \
      --fps 30 \
      --root data \
      --repo-id ${HF_USER}/eval_aloha_test \
      --tags tutorial eval \
      --warmup-time-s 5 \
      --episode-time-s 30 \
      --reset-time-s 30 \
      --num-episodes 10 \
      -p outputs/train/act_aloha_test/checkpoints/last/pretrained_model

You can visualize the evaluation dataset afterward using:

.. code-block:: bash

   python lerobot/scripts/visualize_dataset.py \
      --root data \
      --repo-id ${HF_USER}/eval_aloha_test

