FROM ros:noetic

# Install common dependencies and tools
RUN apt-get update && apt-get install -y \
    ros-noetic-rosbridge-suite \
    ros-noetic-tf \
    ros-noetic-rviz \
    ros-noetic-mavros \
    ros-noetic-mavros-extras \
    libeigen3-dev \
    libboost-all-dev \
    libceres-dev \
    python3-catkin-tools \
    python3-osrf-pycommon \
    ros-noetic-ros-base \
    ros-noetic-image-common \
    ros-noetic-image-transport-plugins \
    ros-noetic-image-pipeline \
    ros-noetic-usb-cam \
    vim \
    git \
    wget \
    && rm -rf /var/lib/apt/lists/*

# Install GeographicLib datasets for MAVROS
RUN wget https://raw.githubusercontent.com/mavlink/mavros/master/mavros/scripts/install_geographiclib_datasets.sh && \
    chmod +x install_geographiclib_datasets.sh && \
    ./install_geographiclib_datasets.sh && \
    rm install_geographiclib_datasets.sh

# Create a workspace for MAVROS, OpenVINS, and ELP driver in /home/
WORKDIR /home/

# MAVROS setup (already installed via ros-noetic-mavros and ros-noetic-mavros-extras)

# Custom OpenVINS setup with ROS 1 branch
RUN mkdir -p /home/openvins_ws/src && \
    cd /home/openvins_ws/src && \
    git clone https://github.com/jpeixe07/open_vins_ardupilot && \
    cd open_vins_ardupilot && \
    git checkout feat/ros1 && \
    cd /home/openvins_ws && \
    /bin/bash -c "source /opt/ros/noetic/setup.bash; catkin_make"

# ELP Synchronized Stereo Camera
RUN mkdir -p /home/elp_camera_ws/src && \
    cd /home/elp_camera_ws/src && \
    git clone https://github.com/mdkennedy3/elp-synchronized-stereo-camera-ros-pkg && \
    cd /home/elp_camera_ws && \
    /bin/bash -c "source /opt/ros/noetic/setup.bash; catkin_make"

# needs fixing    
RUN echo "source /opt/ros/noetic/setup.bash" >> ~/.bashrc && \
    echo "source /home/openvins_ws/devel/setup.bash" >> ~/.bashrc && \
    echo "source /home/elp_camera_ws/devel/setup.bash" >> ~/.bashrc

# Set entrypoint and default command
ENTRYPOINT ["/ros_entrypoint.sh"]
CMD ["bash"]
