FROM ubuntu:20.04

ARG DEBIAN_FRONTEND=noninteractive

RUN apt-get update
RUN apt-get install -y curl supervisor gnupg

RUN echo "deb http://packages.ros.org/ros/ubuntu focal main" > /etc/apt/sources.list.d/ros-latest.list
RUN curl -s https://raw.githubusercontent.com/ros/rosdistro/master/ros.asc | apt-key add -
RUN apt-get update
RUN apt-get install -y ros-noetic-ros-base
RUN echo "source /opt/ros/noetic/setup.bash" >> /root/.bashrc
RUN /bin/bash -c "source /root/.bashrc"
RUN apt-get install -y python3-rosdep python3-rosinstall python3-rosinstall-generator python3-wstool build-essential
RUN rosdep init
RUN rosdep update
RUN apt-get install -y ros-noetic-rosbridge-suite ros-noetic-xacro ros-noetic-robot-state-publisher

RUN mkdir -p /root/catkin_ws/src
WORKDIR /root/catkin_ws/src
RUN git clone https://github.com/ros-industrial/universal_robot.git
WORKDIR /root/catkin_ws/src/universal_robot
RUN rm -drf CONTRIBUTING.md README.md .travis.yml .gitignore universal_robot* *_moveit_config ur_bringup ur_driver ur_e_description ur_e_gazebo ur_gazebo ur_kinematics ur_msgs
WORKDIR /root/catkin_ws/src
RUN git clone https://github.com/ABC-iRobotics/ur_cycle_start.git
WORKDIR /root/catkin_ws/src/ur_cycle_start
RUN rm -drf docker
WORKDIR /root/catkin_ws

RUN /bin/bash -c '. /opt/ros/noetic/setup.bash; cd /root/catkin_ws; catkin_make'
RUN echo "source /root/catkin_ws/devel/setup.bash" >> /root/.bashrc
RUN /bin/bash -c "source /root/.bashrc"

WORKDIR /root
COPY supervisord.conf /root
ENTRYPOINT ["/usr/bin/supervisord","-c","/root/supervisord.conf"]

EXPOSE 9090/tcp
