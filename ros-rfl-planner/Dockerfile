# Build:
#  docker build --rm -f Dockerfile -t gramaziokohler/ros-rfl-planner .
#
# Usage:
#  docker pull gramaziokohler/ros-rfl-planner

FROM gramaziokohler/ros-moveit
LABEL maintainer "Gonzalo Casas <casas@arch.ethz.ch>"

SHELL ["/bin/bash","-c"]
ENV CATKIN_WS=/root/catkin_ws
RUN mkdir -p $CATKIN_WS/src

# Add robot packages to local catkin workspace
RUN echo "Building RFL Planner for ROS" \
    && source /opt/ros/${ROS_DISTRO}/setup.bash \
    # Update apt-get because its cache is always cleared after installs to keep image size down
    && apt-get update && apt-get -y upgrade \
    # RFL packages
    && git clone -b ${ROS_DISTRO}-devel https://github.com/ros-industrial/abb.git \
    && git clone https://github.com/gramaziokohler/abb_irb4600_40_255.git \
    && git clone https://github.com/gramaziokohler/rfl_description.git \
    # Install dependencies
    && cd $CATKIN_WS \
    && rosdep install -y --from-paths . --ignore-src --rosdistro ${ROS_DISTRO} \
    # Build catkin workspace
    && catkin_make \
    # Clear apt-cache to reduce image size
    && rm -rf /var/lib/apt/lists/*
