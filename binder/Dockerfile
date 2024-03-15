FROM ros:noetic-ros-base-buster
SHELL ["/bin/bash", "-c"] 

USER root
RUN apt update --fix-missing
RUN apt install -y git m4 python python3-pip wget
RUN apt install -y python3-catkin-pkg-modules python3-rospkg-modules libffi-dev
RUN apt update
RUN apt install -y nodejs npm

USER ${NB_USER}
# Install and set path for python dependencies
ENV PYTHONPATH=/opt/ros/noetic/lib/python3/dist-packages:/home/ros/devel/lib/python3/dist-packages:$PYTHONPATH
# RUN python3 -m pip install jupyterlab
RUN pip3 install jupyter
RUN pip3 install --upgrade \
#   sidecar \
#   Cython \
#   openai \
  jupyterlab-git \
  jupyterlab-unfold \
  jupyter-archive \
  jupyterlab-search-replace \
  jupyter-offlinenotebook \
#   jupyter-collaboration \
  jupyter-ai \
  jupyterlab-language-pack-de-DE  \
  jupyterlab_execute_time



# Install jupyter-ros
ENV JUPYTER_ROS = /home/${NB_USER}/workspace/jupyterros
WORKDIR ${JUPYTER_ROS}
RUN git clone https://github.com/RoboStack/jupyter-ros.git
WORKDIR ${JUPYTER_ROS}/jupyter-ros
# RUN sudo apt update
# RUN sudo apt install -y nodejs npm
RUN pip3 install -e .
RUN pip3 install numpy --upgrade
RUN jupyter nbextension install --py --symlink --sys-prefix jupyros
RUN jupyter nbextension enable --py --sys-prefix jupyros

# Install jupyter-zethus
RUN python3 -m pip install jupyterlab-zethus

# Install Knowrob kernel 
RUN pip3 install git+https://github.com/sasjonge/jupyter-knowrob.git
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