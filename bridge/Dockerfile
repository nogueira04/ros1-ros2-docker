# Use the official ROS 2 Foxy image as the base
FROM ros:foxy

# Set the environment variable to avoid interactive prompts during package installations
ENV DEBIAN_FRONTEND=noninteractive

# Add the ROS 1 Noetic package repository
RUN sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros1-latest.list'

# Download and install the ROS key
RUN apt-key adv --keyserver 'hkp://keyserver.ubuntu.com:80' --recv-key C1CF6E31E6BADE8868B172B4F42ED6FBAB17C654

# Set timezone to UTC to avoid tzdata prompt
RUN ln -fs /usr/share/zoneinfo/Etc/UTC /etc/localtime && apt-get install -y tzdata

# Update the package index
RUN apt-get update

# Install ROS 1 Noetic without prompts
RUN apt-get install -y ros-noetic-desktop-full && rm -rf /var/lib/apt/lists/*

RUN apt-get update

# Install necessary dependencies for building from source
RUN apt-get install -y \
    build-essential \
    cmake \
    git \
    python3-colcon-common-extensions \
    python3-pip \
    python3-rosdep2 \
    python3-vcstool \
    python3-argcomplete \
    && rm -rf /var/lib/apt/lists/*

# Initialize rosdep (for dependencies resolution)
RUN rosdep update

# Create a workspace for the bridge
WORKDIR /ros2_ws/src

# Clone the ros1_bridge repository
RUN git clone https://github.com/ros2/ros1_bridge.git -b foxy

# Go to the workspace directory
WORKDIR /ros2_ws

# Install ROS 2 dependencies via rosdep
RUN rosdep install --from-paths src --ignore-src -r -y

RUN apt-get update && apt-get install -y ros-noetic-rosmsg

# First build, skipping ros1_bridge to avoid conflicts
RUN /bin/bash -c "source /opt/ros/foxy/setup.bash && colcon build --symlink-install --packages-skip ros1_bridge"

# Build only ros1_bridge with both environments sourced
RUN /bin/bash -c "source /opt/ros/noetic/setup.bash && source /opt/ros/foxy/setup.bash && colcon build --symlink-install --packages-select ros1_bridge --cmake-force-configure"
# Entry point for the container

ENTRYPOINT ["/ros_entrypoint.sh"]
CMD ["bash"]
