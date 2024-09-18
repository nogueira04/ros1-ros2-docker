FROM ros:foxy


RUN apt-get update && apt-get install -y \
    python3-colcon-common-extensions \
    libeigen3-dev \
    libboost-all-dev \
    libceres-dev \
    ros-foxy-rviz2 \
    ros-foxy-rqt* \
    ros-foxy-cv-bridge \
    ros-foxy-image-transport* \
    && rm -rf /var/lib/apt/lists/*


#RUN rosdep update breaks with priviledged:true

WORKDIR /home/
RUN mkdir -p /home/ros2_ws/src/ && \
    cd /home/ros2_ws/src/ && \
    git clone https://github.com/jpeixe07/open_vins_ardupilot/ && \
    cd open_vins_ardupilot && \
    git checkout feat/ros2 && \
    cd /home/ros2_ws/ && \
    colcon build --event-handlers console_cohesion+ --packages-select ov_core ov_init ov_msckf

#source the workspace
RUN echo "source /opt/ros/foxy/setup.bash" >> ~/.bashrc && \
    echo "source /ros2_ws/install/setup.bash" >> ~/.bashrc 

ENTRYPOINT ["/ros_entrypoint.sh"]
CMD ["bash"]
