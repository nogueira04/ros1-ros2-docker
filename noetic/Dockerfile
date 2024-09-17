FROM ros:noetic

RUN apt-get update && apt-get install -y \
    ros-noetic-rosbridge-suite \
    ros-noetic-tf \
    ros-noetic-rviz \
    && rm -rf /var/lib/apt/lists/*

ENTRYPOINT ["/ros_entrypoint.sh"]
CMD ["bash"]

