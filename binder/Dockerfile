# FROM ros:noetic-ros-base-buster
FROM intel4coro/base-notebook:20.04-noetic-full-xpra

SHELL ["/bin/bash", "-c"] 

USER root
RUN apt update --fix-missing
RUN apt install -y git m4 wget
# RUN apt install -y git m4 python python3-pip wget
RUN apt install -y python3-catkin-pkg-modules python3-rospkg-modules libffi-dev
RUN apt update
RUN apt install -y nodejs npm

USER ${NB_USER}
# Install and set path for python dependencies
# ENV PYTHONPATH=/opt/ros/noetic/lib/python3/dist-packages:/home/ros/devel/lib/python3/dist-packages:$PYTHONPATH
RUN pip uninstall -y jlab-enhanced-cell-toolbar
RUN pip install https://raw.githubusercontent.com/yxzhan/jupyterlab-rviz/master/dist/jupyterlab_rviz-0.3.2.tar.gz \
  https://raw.githubusercontent.com/yxzhan/extension-examples/main/cell-toolbar/dist/jupyterlab_examples_cell_toolbar-0.1.4.tar.gz
RUN mamba install -y ripgrep
RUN pip install --upgrade \
  sidecar \
  Cython \
  openai \
  jupyterlab-git \
  jupyterlab-unfold \
  jupyter-archive \
  jupyterlab-search-replace \
  jupyter-offlinenotebook \
  jupyter-collaboration \
  jupyter-ai \
  jupyterlab-language-pack-de-DE  \
  jupyterlab_execute_time

USER root
# Install jupyter-ros
ENV JUPYTER_ROS = /home/${NB_USER}/workspace/jupyterros
WORKDIR ${JUPYTER_ROS}
RUN git clone https://github.com/RoboStack/jupyter-ros.git
WORKDIR ${JUPYTER_ROS}/jupyter-ros
# RUN sudo apt update
# RUN sudo apt install -y nodejs npm
RUN pip install -e .
USER ${NB_USER}
RUN pip install numpy --upgrade
RUN jupyter nbextension install --py --symlink --sys-prefix jupyros
RUN jupyter nbextension enable --py --sys-prefix jupyros

# Install jupyter-zethus
RUN python3 -m pip install jupyterlab-zethus

# Install Knowrob kernel 
RUN pip install git+https://github.com/sasjonge/jupyter-knowrob.git
WORKDIR ~/.local/share/jupyter/kernels/
USER root
RUN mkdir jknowrob
USER ${NB_USER}
RUN wget https://raw.githubusercontent.com/sasjonge/jupyter-knowrob/master/kernel.json -P ~/.local/share/jupyter/kernels/jknowrob

# We need the json_prolog_msgs to communicate with KnowRob
WORKDIR ${JUPYTER_ROS}/src
RUN /usr/bin/python /opt/ros/noetic/bin/catkin_init_workspace
RUN source /opt/ros/noetic/setup.bash
RUN git clone https://github.com/code-iai/iai_common_msgs.git

WORKDIR ${JUPYTER_ROS}
# Build the catkin workspace
ENV CMAKE_PREFIX_PATH=/home/${NB_USER}/knowrob_ws/devel:/opt/ros/noetic
RUN /opt/ros/noetic/bin/catkin_make -DPYTHON_EXECUTABLE=/usr/bin/python3

WORKDIR /lectures

# Configure container startup
# COPY run_notebook.sh /run_notebook.sh
# COPY /knowrob_cloud /home/ros/src/knowrob_cloud
# ENTRYPOINT ["/run_notebook.sh"]