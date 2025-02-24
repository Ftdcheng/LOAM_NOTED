FROM ros:noetic AS build

ENV LANG=C.UTF-8
ENV LC_ALL=C.UTF-8
ENV ROS_DISTRO=noetic
ENV DEBIAN_FRONTEND=noninteractive

RUN sed -i 's|http://archive.ubuntu.com/ubuntu/|https://mirrors.tuna.tsinghua.edu.cn/ubuntu/|g' /etc/apt/sources.list && \
    sed -i 's|http://security.ubuntu.com/ubuntu/|https://mirrors.tuna.tsinghua.edu.cn/ubuntu/|g' /etc/apt/sources.list

RUN apt-get update && \
    apt install -y git curl python3-colcon-common-extensions python3 python3-pip libpcl-dev p7zip-full && \
    pip3 install rosdepc && \
    rosdepc init && rosdepc update && \
    rm -rf /var/lib/apt/lists/*

COPY opencv-2.4.13.6.zip /tmp
WORKDIR /tmp
RUN 7za x opencv-2.4.13.6.zip && \
    cd opencv-2.4.13.6 && \
    mkdir build && cd build && \
    cmake ..
#     cmake .. && make -j8 && make install && \
#     rm -rf /tmp/opencv*

# WORKDIR /root/loam_ws/src
# RUN git clone https://github.com/cuitaixiang/LOAM_NOTED && \
#     cd LOAM_NOTED/src/ && \
#     rosdepc install --from loam_velodyne --ignore-src -r -y

# WORKDIR /root/loam_ws
# RUN bash /opt/ros/${ROS_DISTRO}/setup.bash && \
#     catkin_make

# FROM ros:noetic

# # 将构建好的工作空间从构建阶段复制到最终镜像
# COPY --from=build /root/loam_ws /root/loam_ws
# ENV LANG=C.UTF-8
# ENV LC_ALL=C.UTF-8
# ENV ROS_DISTRO=noetic
# ENV DEBIAN_FRONTEND=noninteractive

# RUN sed -i 's|http://archive.ubuntu.com/ubuntu/|https://mirrors.tuna.tsinghua.edu.cn/ubuntu/|g' /etc/apt/sources.list && \
#     sed -i 's|http://security.ubuntu.com/ubuntu/|https://mirrors.tuna.tsinghua.edu.cn/ubuntu/|g' /etc/apt/sources.list

# RUN /bin/sh -c echo 'Etc/UTC' > /etc/timezone && \
#     ln -s /usr/share/zoneinfo/Etc/UTC /etc/localtime && \
#     apt-get update && \
#     apt-get install -q -y --no-install-recommends tzdata && \
#     rm -rf /var/lib/apt/lists/*
# COPY ros_enterpoint.sh /ros_enterpoint.sh
# ENTRYPOINT [ "/bin/bash", "/ros_enterpoint.sh" ]
# CMD ["bash"]