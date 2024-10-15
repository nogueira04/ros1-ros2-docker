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
    meson \
    ninja-build \
    pkg-config \
    gcc \
    g++ \
    systemd \
    python3-pip \
    python3-dev \
    python3-opencv \
    python3-wxgtk4.0 \
    python3-matplotlib \
    python3-lxml \
    python3-pygame \
    && rm -rf /var/lib/apt/lists/*

# Install GeographicLib datasets for MAVROS
RUN wget https://raw.githubusercontent.com/mavlink/mavros/master/mavros/scripts/install_geographiclib_datasets.sh && \
    chmod +x install_geographiclib_datasets.sh && \
    ./install_geographiclib_datasets.sh && \
    rm install_geographiclib_datasets.sh

# Create a workspace for OpenVINS, and ELP driver in /home/
WORKDIR /home/

# Custom OpenVINS setup with ROS 1 branch
RUN mkdir -p /home/openvins_ws/src && \
    cd /home/openvins_ws/src && \
    rm -rf /homeo/openvins_ws/src/open_vins_ardupilot && \
    git clone https://github.com/jpeixe07/open_vins_ardupilot && \
    cd open_vins_ardupilot && \
    git checkout feat/ros1 && \
    cd /home/openvins_ws && \
    /bin/bash -c "source /opt/ros/noetic/setup.bash; catkin_make"

# ELP Synchronized Stereo Camera
RUN mkdir -p /home/elp_camera_ws/src && \
    cd /home/elp_camera_ws/src && \
    rm -rf /home/elp_camera_ws/src/elp-synchronized-stereo-camera-ros-pkg && \
    git clone https://github.com/nogueira04/elp-synchronized-stereo-camera-ros-pkg && \
    cd /home/elp_camera_ws && \
    /bin/bash -c "source /opt/ros/noetic/setup.bash; catkin_make"

# Install Mavlink Router
RUN git clone https://github.com/mavlink-router/mavlink-router.git /home/mavlink-router && \
    cd /home/mavlink-router && \
    git submodule update --init --recursive && \
    pip3 install meson && \
    meson setup build . && \
    ninja -C build && \
    sudo ninja -C build install

# MAVProxy setup
RUN pip3 install PyYAML mavproxy --user && \
    echo "export PATH=$PATH:/root/.local/bin" >> ~/.bashrc

# Source ROS and workspaces
RUN echo "source /opt/ros/noetic/setup.bash" >> ~/.bashrc && \
    echo "source /home/openvins_ws/devel/setup.bash" >> ~/.bashrc && \
    echo "source /home/elp_camera_ws/devel/setup.bash" >> ~/.bashrc

# Set entrypoint and default command
ENTRYPOINT ["/ros_entrypoint.sh"]
CMD ["bash"]
