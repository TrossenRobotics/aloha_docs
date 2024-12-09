=======================
Training and Evaluation
=======================


Virtual Environment Setup
=========================

Effective containerization is important when it comes to running machine learning models as there can be conflicting dependencies.
You can either use a Virtual Environment or Conda.

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



Train and Evaluate
==================

Depending upon the setup you are using 

.. toctree::
  :maxdepth: 2

  ./training/mobile.rst
  ./training/stationary.rst
  ./training/solo.rst


Hugging Face for Dataset storage
================================

To understand how to utilize Hugging Face for Uploading and Downloading Datasets

.. toctree::
  :maxdepth: 2
  
  ./training/hugging_face.rst

LeRobot Guide for Aloha
=======================

To understand how to utilize LeRobot for Aloha use the below guide 

.. toctree::
  :maxdepth: 2

  ./training/lerobot_guide.rst
