==================
Hugging Face Guide
==================

Uploading and Downloading Datasets on Hugging Face
==================================================

Creating an Account
-------------------

If you don't already have an account, sign up for a new account on the `Hugging Face Sign Up <https://huggingface.co/join>`_.

Creating a New Dataset Repository
---------------------------------

Web Interface
^^^^^^^^^^^^^
#. Navigate to the `Hugging Face website <https://huggingface.co>`_.
#. Log in to your account.
#. Click on your profile picture in the top-right corner and select "New dataset."
#. Follow the on-screen instructions to create a new dataset repository.

Command Line Interface (CLI)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^
   #. Ensure you have the `huggingface_hub <https://huggingface.co/docs/huggingface_hub/index>`_ library installed.
   #. Use the following Python script to create a new repository:

    .. code-block:: python

        from huggingface_hub import HfApi
        api = HfApi()

        api.create_repo(repo_id="username/repository_name", repo_type="dataset")

For more information on creating repositories, refer to the `Hugging Face Repositories <https://huggingface.co/docs/hub/repositories>`_.

Uploading Your Dataset
----------------------

You have two primary methods to upload datasets: through the web interface or using the Python API.

Web Interface
^^^^^^^^^^^^^

#. Navigate to your dataset repository on the Hugging Face website.
#. Click on the "Files and versions" tab.
#. Drag and drop your dataset files into the files section.
#. Click "Commit changes" to save the files in the repository.

Python API
^^^^^^^^^^

You can use the following Python script to upload your dataset:

    .. code-block:: python

        from huggingface_hub import HfApi
        api = HfApi()

        api.upload_folder(
            folder_path="path/to/dataset",
            repo_id="username/repository_name",
            repo_type="dataset",
        )

**Example**:

    .. code-block:: python

        from huggingface_hub import HfApi
        api = HfApi()

        api.upload_folder(
            folder_path="~/aloha_data/aloha_stationary_block_pickup",
            repo_id="TrossenRoboticsCommunity/aloha_static_datasets",
            repo_type="dataset",
        )

For more information on uploading datasets, refer to the `Hugging Face Uploading <https://huggingface.co/docs/hub/upload>`_.

Downloading Datasets
--------------------

You can download datasets either by cloning the repository or using the Hugging Face CLI.

Cloning the Repository
^^^^^^^^^^^^^^^^^^^^^^

To clone the repository, use the following command:

    .. code-block:: bash

        $ git clone https://huggingface.co/datasets/username/repository_name

Using the Hugging Face CLI
^^^^^^^^^^^^^^^^^^^^^^^^^^

You can also use the Hugging Face CLI to download datasets with the following Python script:

    .. code-block:: python

        from huggingface_hub import snapshot_download

        # Download the dataset
        snapshot_download(
            repo_id="username/repository_name",
            repo_type="dataset",
            local_dir="path/to/local/directory",
            allow_patterns="*.hdf5"
        )

.. note::

   - The dataset episodes are stored in ``.hdf5`` format. Therefore, ensure that you only allow these patterns during download.

For more information on downloading datasets, refer to the `Hugging Face Datasets <https://huggingface.co/docs/hub/download>`_.

Additional Information
----------------------

- **Repository Management**: Utilize the `Hugging Face Hub documentation <https://huggingface.co/docs/hub/repositories>`_ for detailed instructions on managing repositories, handling versions, and setting permissions.
- **Dataset Formats**: Hugging Face supports various dataset formats. For this guide, we specifically use the Aloha's native ``.hdf5`` format.
- **Community Support**: If you encounter any issues, refer to the `Hugging Face community forums <https://discuss.huggingface.co>`_ for additional support.

By following this guide, you should be able to seamlessly upload and download datasets using the Hugging Face platform. For more detailed guides and examples, refer to the `Hugging Face Documentation <https://huggingface.co/docs>`_.
