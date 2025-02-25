FROM ros:noetic AS build

ENV LANG=C.UTF-8
ENV LC_ALL=C.UTF-8
ENV ROS_DISTRO=noetic
ENV DEBIAN_FRONTEND=noninteractive
RUN sh -c '. /etc/lsb-release && echo "deb http://mirrors.tuna.tsinghua.edu.cn/ros/ubuntu/ `lsb_release -cs` main" > /etc/apt/sources.list.d/ros-latest.list' && \
    sed -i 's|http://archive.ubuntu.com/ubuntu/|https://mirrors.tuna.tsinghua.edu.cn/ubuntu/|g' /etc/apt/sources.list && \
    sed -i 's|http://security.ubuntu.com/ubuntu/|https://mirrors.tuna.tsinghua.edu.cn/ubuntu/|g' /etc/apt/sources.list

RUN apt-get update && \
    apt install -y p7zip-full && \
    rm -rf /var/lib/apt/lists/*

COPY opencv-4.3.0.zip /tmp
WORKDIR /tmp
RUN 7za x opencv-4.3.0.zip && \
    cd opencv-4.3.0 && \
    mkdir build && cd build && \
    cmake .. && make -j8 && make install && \
    rm -rf /tmp/opencv*

RUN apt-get update && \
    apt install -y git curl python3-colcon-common-extensions python3 python3-pip ros-noetic-pcl-conversions ros-noetic-tf ros-noetic-tf2 libpcl-dev p7zip-full  && \
    pip3 install rosdepc && \
    rosdepc init && rosdepc update && \
    rm -rf /var/lib/apt/lists/*

WORKDIR /root/loam_ws/src
RUN git clone https://github.com/Ftdcheng/LOAM_NOTED.git && \
    cd LOAM_NOTED && \
    git checkout feature_newlib && \
    cd src/ && \
    rosdepc install --from-paths loam_velodyne  -y --ignore-src -r

WORKDIR /root/loam_ws
RUN bash -c ". /opt/ros/${ROS_DISTRO}/setup.bash && \
    catkin_make"

RUN echo ". /opt/ros/${ROS_DISTRO}/setup.bash" >> /root/.bashrc

FROM ros:noetic AS runtime

# 将构建好的工作空间从构建阶段复制到最终镜像
COPY --from=build /root/loam_ws /root/loam_ws
ENV LANG=C.UTF-8
ENV LC_ALL=C.UTF-8
ENV ROS_DISTRO=noetic
ENV DEBIAN_FRONTEND=noninteractive

RUN sed -i 's|http://archive.ubuntu.com/ubuntu/|https://mirrors.tuna.tsinghua.edu.cn/ubuntu/|g' /etc/apt/sources.list && \
    sed -i 's|http://security.ubuntu.com/ubuntu/|https://mirrors.tuna.tsinghua.edu.cn/ubuntu/|g' /etc/apt/sources.list

RUN /bin/sh -c echo 'Etc/UTC' > /etc/timezone && \
    apt-get update && \
    apt-get install -q -y --no-install-recommends tzdata && \
    rm -rf /var/lib/apt/lists/*
RUN echo ". /opt/ros/${ROS_DISTRO}/setup.bash" >> /root/.bashrc
COPY ros_enterpoint.sh /ros_enterpoint.sh
ENTRYPOINT [ "/bin/bash", "/ros_enterpoint.sh" ]
CMD ["bash"]