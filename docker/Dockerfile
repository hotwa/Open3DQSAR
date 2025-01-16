# 第一阶段：构建阶段
FROM ubuntu:22.04 AS builder

# 设置环境变量
ENV DEBIAN_FRONTEND=noninteractive

# 更新系统、安装依赖并清理缓存
RUN apt update && apt upgrade -y && \
    apt install -y \
    build-essential \
    gcc \
    g++ \
    gfortran \
    cmake \
    make \
    wget \
    tar \
    libatlas-base-dev \
    liblapacke-dev \
    libblas-dev \
    git \
    libedit-dev \
    zlib1g-dev \
    openbabel \
    libeigen3-dev \
    automake \
    autoconf \
    libtool \
    vim \
    gnuplot \
    pymol \
    && ln -s /usr/bin/obabel /usr/bin/babel \
    && apt clean \
    && rm -rf /var/lib/apt/lists/*

# 克隆 Open3DQSAR 仓库
RUN git clone https://github.com/UnixJunkie/Open3DQSAR

# 进入 Open3DQSAR 目录并构建
WORKDIR /Open3DQSAR/open3dqsar
RUN ./bootstrap && \
    ./configure --with-atlas && \
    make -j$(nproc) && \
    make install

# 第二阶段：运行阶段
FROM ubuntu:22.04

# 设置环境变量
ENV DEBIAN_FRONTEND=noninteractive \
    O3_BABEL_PATH=/usr/bin \
    O3_GNUPLOT=/usr/bin/gnuplot \
    O3_PYMOL=/usr/bin/pymol \
    O3_TEMP_PATH=/tmp \
    O3_QM_ENGINE=/usr/local/bin/orca \
    O3_MD_GRID_PATH=/usr/bin/mdgrid \
    O3_CS3D=/usr/bin/cs3d

# 安装运行时依赖
RUN apt update && apt install -y \
    libatlas-base-dev \
    liblapacke-dev \
    libblas-dev \
    openbabel \
    gnuplot \
    pymol \
    && ln -s /usr/bin/obabel /usr/bin/babel \
    && apt clean \
    && rm -rf /var/lib/apt/lists/*

# 从构建阶段复制构建结果
COPY --from=builder /usr/local/bin /usr/local/bin
COPY --from=builder /Open3DQSAR /Open3DQSAR

# 进入测试目录并运行测试脚本
WORKDIR /Open3DQSAR/open3dqsar/test
RUN ./test.sh > test.log 2>&1 &

# 设置容器启动时的默认命令（保持容器运行）
ENTRYPOINT ["tail", "-f", "/dev/null"]