FROM ros:foxy

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update && apt-get install -y \
    python3-colcon-common-extensions \
    && rm -rf /var/lib/apt/lists/*

RUN rosdep update

WORKDIR /ros2_ws
RUN mkdir -p src

ENTRYPOINT ["/ros_entrypoint.sh"]
CMD ["bash"]
