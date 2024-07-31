=======================
Training and Evaluation
=======================



Virtual Environment Setup
=========================

Effective containerization is important when it comes to running machine learning models as there can be conflicting dependencies. You can either use a Virtual Environment or Conda.

Virtual Environment Installation and Setup
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

#. Install the virtual environment package:

  .. code-block:: bash

      $ sudo apt-get install python3-venv

#. Create a virtual environment:

  .. code-block:: bash

      $ python3 -m venv ~/act  # Creates a venv "act" in the home directory, can be created anywhere

#. Activate the virtual environment:

  .. code-block:: bash

      $ source act/bin/activate

Conda Setup
^^^^^^^^^^^

#. Create a virtual environment:

  .. code-block:: bash

    $ conda create -n aloha python=3.8.10

#. Activate the virtual environment:

  .. code-block:: bash

    $ conda activate aloha

Install Dependencies
^^^^^^^^^^^^^^^^^^^^

Install the necessary dependencies inside your containerized environment:

.. code-block:: bash

  $  pip install dm_control==1.0.14
  $  pip install einops
  $  pip install h5py
  $  pip install ipython
  $  pip install matplotlib
  $  pip install mujoco==2.3.7
  $  pip install opencv-python
  $  pip install packaging
  $  pip install pexpect
  $  pip install pyquaternion
  $  pip install pyyaml
  $  pip install rospkg
  $  pip install torch
  $  pip install torchvision

Clone Repository
================

Clone ACT if using Aloha Stationary

.. code-block:: bash

    $ git clone https://github.com/Interbotix/act.git act_training_evaluation


Clone ACT++ if using Aloha Mobile

.. code-block:: bash

    $ git clone https://github.com/Interbotix/act_plus_plus.git act_training_evaluation


Build and Install ACT Models
============================

.. code-block:: bash
   :emphasize-lines: 4

    ├── act
    │   ├── assets
    │   ├── constants.py
    │   ├── detr
    │   ├── ee_sim_env.py
    │   ├── imitate_episodes.py
    │   ├── __init__.py
    │   ├── policy.py
    │   ├── record_sim_episodes.py
    │   ├── scripted_policy.py
    │   ├── sim_env.py
    │   ├── utils.py
    │   └── visualize_episodes.py
    ├── COLCON_IGNORE
    ├── conda_env.yaml
    ├── LICENSE
    └── README.md


Navigate to the ``detr`` directory inside the repository and install the detr module whihc contains the model definitions using the below command:

.. code-block:: bash

    $ cd /path/to/act/detr && pip install -e .

Training
========

To start the training, follow the steps below:

#. Sanity Check: 

    Ensure you have all the hdf5 episodes located in the correct folder after following the data collection steps :ref:`operation/data_collection:Task Creation`.

#. Source ROS Environment:

   .. code-block:: bash

      $ source /opt/ros/humble/setup.bash
      $ source interbotix_ws/install/setup.bash

#. Activate Virtual Environment:

   .. code-block:: bash

      $ source act/bin/activate

#. Start Training

   .. code-block:: bash

      $ cd /path/to/act/repository/
      $ python3 imitate_episodes.py \
        --task_name aloha_stationary_dummy \
        --ckpt_dir <ckpt dir> \
        --policy_class ACT \
        --kl_weight 10 \
        --chunk_size 100 \
        --hidden_dim 512 \
        --batch_size 8 \
        --dim_feedforward 3200 \
        --num_epochs 2000 \
        --lr 1e-5 \
        --seed 0

.. tip::

   - ``task_name`` argument should match one of the task names in the ``TASK_CONFIGS``, as configured in the :ref:`operation/data_collection:Task Creation` section.
   - ``ckpt_dir``: The relative location where the checkpoints and best policy will be stored.
   - ``policy_class``: Determines the choice of policy 'ACT'/'CNNMLP'.
   - ``kl_weight``: Controls the balance between exploration and exploitation.
   - ``chunk_size``: Determines the length of the action sequence. K=1 is no action chunking and K=episode length is full open loop control.
   - ``batch_size``: Low batch size leads to better generalization and high batch size results in slower convergence but faster training time.
   - ``num_epochs``: Too many epochs lead to overfitting; too few epochs may not allow the model to learn.
   - ``lr``: Higher learning rate can lead to faster convergence but may overshoot the optima, while lower learning rate might lead to slower but stable optimization.

We recommend the following parameters:

.. list-table::
   :align: center
   :widths: 25 75
   :header-rows: 1

   * - Parameter
     - Value
   * - Policy Class
     - ACT
   * - KL Weight
     - 10
   * - Chunk Size
     - 100
   * - Batch Size
     - 2
   * - Num of Epochs
     - 3000
   * - Learning Rate
     - 1e-5

Evaluation
==========

To evaluate a trained model, follow the steps below:

#. Bring up the ALOHA 

   - Stationary: :ref:`operation/stationary:Running ALOHA Bringup`
   - Mobile: :ref:`operation/mobile:Running ALOHA Bringup`

#. Configure the environment

   .. code-block:: bash

       $ source /opt/ros/humble/setup.bash  # Configure ROS system install environment
       $ source interbotix_ws/install/setup.bash  # Configure ROS workspace environment
       $ source /<path_to_aloha_venv>/bin/activate  # Configure ALOHA Python environment
       $ cd ~/<act_repository>/act/

#. Run the evaluation script

   .. code-block:: bash   
      :emphasize-lines: 13-14

       python3 imitate_episodes.py \
        --task_name aloha_stationary_dummy \
        --ckpt_dir <ckpt dir> \
        --policy_class ACT \
        --kl_weight 10 \
        --chunk_size 100 \
        --hidden_dim 512 \
        --batch_size 8 \
        --dim_feedforward 3200 \
        --num_epochs 2000 \
        --lr 1e-5 \
        --seed 0 \
        --eval \
        --temporal_agg

.. note::

   - The ``task_name`` argument should match one of the task names in the ``TASK_CONFIGS``, as configured in the :ref:`operation/data_collection:Task Creation` section.
   - The ``ckpt_dir`` argument should match the correct relative directory location of the trained policy.
   - The ``eval`` flag will set the script into evaluation mode.
   - The ``temporal_agg`` is not required, but helps to smoothen the trajectory of the robots.