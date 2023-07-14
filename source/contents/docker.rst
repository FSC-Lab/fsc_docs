Docker
******

Installation
============

#. Uninstall any preexisisting installation of docker by using

   .. code-block:: bash

      sudo apt-get remove docker-desktop

   .. tip:: 
      To completely clean up all remaining configuration and data from a previous
      installation of Docker, you can run

      .. code-block:: bash

         rm -r $HOME/.docker/desktop
         sudo rm /usr/local/bin/com.docker.cli
         sudo apt purge docker-desktop


#. Install a few key packages

   .. code-block:: bash

      sudo apt-get update
      sudo apt-get install ca-certificates curl gnupg

#. Obtain a copy of Docker's signing key

   .. code-block:: bash

      sudo install -m 0755 -d /etc/apt/keyrings
      curl -fsSL https://download.docker.com/linux/debian/gpg | \
      sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
      sudo chmod a+r /etc/apt/keyrings/docker.gpg

#. Add Docker's repository to your sources list for your Ubuntu release

   .. code-block:: bash

      echo \
          "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/debian \
          "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
      sudo tee /etc/apt/sources.list.d/docker.list > /dev/null                

#. Download Docker's ``.deb`` package from `Docker's releases page <https://docs.docker.com/desktop/release-notes/>`__ with
   sections that look similar to the following

   .. image:: deb_download.png
      :alt: The downloads for Docker 4.19.0.
      :width: 400


   Click on the *Debian* link to start the download. You will obtain a
   file named like ``docker-desktop-{{version}}-{{arch}}.deb``, e.g.
   ``docker-desktop-4.19-amd64.deb``

#. Finally, you can update and install Docker

   .. code-block:: bash

      sudo apt-get update              
      sudo apt-get install ./docker-desktop-{{version}}-{{arch}}.deb

   .. note::
      You must run the last command above from the folder where
      you downloaded ``docker-desktop-{{version}}-{{arch}}.deb`` earlier

Post-Install Steps
------------------

#. Add your user to the ``docker`` group. 

   .. warning::
      If this step is neglected, you
      must run the ``docker`` command as ``sudo``

   Create the ``docker`` group in case it does not exist

   .. code-block:: bash

      sudo groupadd docker

   Then add your user to it

   .. code-block:: bash

      sudo usermod -aG docker $USER

#. Configure the ``docker`` service to start on boot by running

   .. code-block:: bash

      sudo systemctl enable docker.service
      sudo systemctl enable containerd.service

   then either reboot the machine to start the service (upon bootup, as
   configured above), or immediately start the service via

   .. code-block:: bash

      sudo service docker start

Hello World
-----------

Test your fresh ``docker`` installation by running

.. code-block:: bash

   docker run hello-world 


Images
======

A key concept of containerization is **images**. 
A docker **image** defines a set of libraries, tools, and other dependencies required to launch an application.
From an image, one or more **containers** can be created. 
From within a container, an application is launched.

Many developers created images for their own applications and hosted them on `Docker Hub <https://hub.docker.com/>`__.
To obtain their images, run

.. code-block:: bash

   docker pull {{image-name}}

Example of Docker Images
------------------------

A list of useful docker images are listed here

* Ubuntu: ``ubuntu/{{release codename}}`` 
* ROS: ``ros/{{ros codename}}-ros-core``
* Tensorflow: ``tensorflow/tensorflow:latest-gpu``
* PyTorch: 
   * Mainline ``pytorch/pytorch``
   * For AMD GPUs: ``rocm/pytorch``
* Yolo: ``ultralytics/ultralytics``
* Nvidia tensorflow: ``nvcr.io/nvidia/tensorrt``

Image management
----------------

#. To inspect docker images you have pulled to your machine

   .. code-block:: bash 

      docker image ls

#. To remove docker images from your machine

   .. code-block:: bash

      docker image rm {{image}}

#. To rename / re-*tag* an image

   .. code-block:: bash

      docker tag {{source}} {{target}}

   .. tip::

      A docker image's name may consist of three main components ``namespace/name:tag``.
      The *namespace* identifies the owner/developer of the image, and the *tag* may identify revisions or variants of the image

      Use ``docker tag`` to add a *namespace* to an image for it to be publicly shared, e.g. pushed to docker hub

      .. code-block:: bash

         docker tag my_image:latest my_name/my_image:release

#. To push an image to Docker hub

   .. code-block:: bash

      docker push {{image}}

.. warning::

   You must run ``docker login`` and enter your docker hub username/password before you can push your image to your account (or pull a private image)

Containers
==========

A container provides the actual environment inside which an application is run or development work is done.

Running a container
-------------------

A container is launched using the ``docker run`` command.
In its simplest form, this command is like

.. code-block:: bash

   docker run {{image}}


Anatomy of docker run
---------------------

In practice, the ``docker run`` incantation tends to be much more complex.
This section will introduce the parameters for ``docker run`` by an example

.. code-block:: bash

   docker run -it --privileged \
     --gpus all \
     -e "DISPLAY=$DISPLAY" \
     -v "$HOME/Documents/data_dir:/root/data_dir:rw" \
     -v "$HOME/.Xauthority:/root/.Xauthority:rw" \
     -v "/tmp/.X11-unix:/tmp/.X11-unix" \
     -p 8888:8888 \
     -p 6006:6006 \
     --name learning-dev \
     fsc_lab/tensorflow:contraction_analysis

The parameters in this incantation are respectively

* ``-it`` (Shorthand for ``--interactive`` and ``--tty``)

   Lets you access the container *interactively* and opens a terminal for you to do so.

* ``--privileged``

   Lets you do everything inside the container as if you are on your host computer.

   .. warning::
      Setting ``--privileged`` is a severe escalation of privileges that is rarely justified **unless** you need to run GUI applications inside the container.

* ``--gpus all``

   Grants access to all your GPUs from the container **AFTER** ``nvidia-docker`` and associated runtimes have been set-up

* ``-v "$HOME/Documents/data_dir:/root/data_dir:rw"``

   Mounts a directory from your host machine, such that you gain access (controlled by ``:rw`` read-write, alternately ``ro`` read-only) to it inside the container

* ``-e "DISPLAY=$DISPLAY"`` through ``-v "$HOME/Documents/data_dir:/root/data_dir:rw"``
   
   Sets the ``DISPLAY`` environment variable and mounts device files to let GUI applications inside the container, ranging from ``firefox`` to a lowly Python program running ``plt.show()``, to display on the host machine

* ``-p 8888:8888`` and ``-p 6006:6006``

   Exposes network ports from the container. Jupyter notebook starts a server on ``8888``, while Tensorboard starts a server on ``6666``. 

   .. tip::

      Use ``--net host`` to activate *host networking mode*, exposing ALL ports in the container and sidestepping the need to set ``-p`` options individually.

      This has the potential downside that the ports opened by apps in your container may collide by those opened by the host machine

* ``--name learning_dev``

   Names the container such that you can start and stop it by name. If not specified, docker will allocate a nonsensical ``{{adjective}}-{{noun}}`` name to the container.

The example above shows a typical ``docker run`` incantation to launch a container for interactive development.
The following example shows how to launch a specific app inside a container

.. code-block:: bash

   docker run -it --rm \
     -v "$HOME/Documents/data_dir:/root/data_dir:rw" \
     ultralytics/ultralytics \
     'yolo detect train data=/root/data_dir/my_data.yaml model=yolov8n.pt'

The parameters in this incantation are respectively

* ``-it``
  
  Still useful for allocating a terminal to let you see the program output

* ``--rm``

   The container's job is done after the program exits. Remove it afterwards.

   Since you are not holding on the container for interactive development, presence of ``--rm`` often means ``--name`` is unncessary

* ``'yolo detect ...'``
   
   Command that starts the specific application in this container.

Advanced
''''''''

A container must be tailored to let you act as your host user inside the container. 
This is critical for operations tied to your user, e.g. writing to files inside ``-v`` mounted volumes, running ``git commit`` and ``git push``, etc.

A entrypoint script for setting user-privileges, with `contents such as the following <https://gist.github.com/alexpearce/b438bc9f358ba7b333f2e15e6bd826f0>`__, must be provided by the container

.. code-block:: bash

   #!/usr/bin/bash
   USER_ID=${LOCAL_USER_ID:-9001}
   GROUP_ID=${LOCAL_GROUP_ID:-$USER_ID}

   echo "Starting with UID : $USER_ID, GID: $GROUP_ID"
   groupadd -g $GROUP_ID usergroup
   useradd --shell /bin/bash -u $USER_ID -g usergroup -o -c "" -m user
   export HOME=/home/user

   exec /usr/local/bin/gosu user:usergroup "$@"

Then, the options `-e "LOCAL_USER_ID=$(id -u)"` and `-e "LOCAL_GROUP_ID=$(id -g)"` must be supplied to `docker run`.


Container Workflow
--------------------

#. To exit a interactive container (started with ``-it``)

   * Stopping the container: Press ``CTRL-d``
   * Without stopping the container (Detaching): Press ``CTRL-p CTRL-q``

#. To re-attach (access to the terminal input/output) of an detached container

   .. code-block:: bash
      
      docker attach {{container_name}}

#. To start a container

   .. code-block:: bash
      
      docker start {{container_name}}

#. To stop a container

   .. code-block:: bash

      docker stop {{container_name}}
   
   add ``-f`` to force removal

#. To run a command in a container

   .. code-block:: bash

      docker exec {{container_name}} {{command}}

   A ubiquitous incantation of this command is

   .. code-block:: bash

      docker exec -it {{container_name}} bash

   which starts a new terminal in the container

#. To remove a stopped container

   .. code-block:: bash

      docker rm {{container_name}}
