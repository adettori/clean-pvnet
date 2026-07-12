FROM pytorch/pytorch:1.1.0-cuda10.0-cudnn7.5-devel

# Hack to not have tzdata cmdline config during build
RUN ln -fs /usr/share/zoneinfo/Europe/Amsterdam /etc/localtime

RUN gpg --keyserver keyserver.ubuntu.com --recv-keys A4B469963BF863CC
RUN gpg --export --armor A4B469963BF863CC | apt-key add

# Prepare apt
RUN apt-get update && \
    apt install -yq software-properties-common && \
    apt update
    
# Install python 3 dependencies
RUN apt-get install -yq \
        wget \
        build-essential \
        libffi-dev \
        libpng-dev \
        libeigen3-dev \
        libglfw3-dev \
        libglfw3 \
        libgoogle-glog-dev \
        libsuitesparse-dev \
        libatlas-base-dev \
        libreadline-gplv2-dev \
        libncursesw5-dev \
        libssl-dev \
        libsqlite3-dev \
        tk-dev \
        libgdbm-dev \
        libc6-dev \
        libbz2-dev 

# Install python 3.7
RUN wget https://www.python.org/ftp/python/3.7.4/Python-3.7.4.tgz && \
    tar xzf Python-3.7.4.tgz && \
    cd Python-3.7.4 && \
    ./configure && \
    make && \
    make install && \
    cd .. && \
    rm -r Python-3.7.4 Python-3.7.4.tgz

# Create symlink for python 3.7
RUN rm /usr/bin/python3 && ln -s /usr/local/bin/python3.7 /usr/bin/python3

# Fix lsb_release issue by replacing first line with python2 reference
RUN VAR="#! /usr/bin/python2.7" sed -i "1s/.*/$VAR/" /usr/bin/lsb_release

# Install dependencies for project
RUN apt install -yq \
        nano \
        sudo \
        wget \
        curl \
        cmake \
        ninja-build \
        git \
        ca-certificates \
        libjpeg-dev \
        zip \
        unzip \
        libpng-dev \
        libeigen3-dev \
        libglfw3-dev \
        libglfw3 \
        libgoogle-glog-dev \
        libsuitesparse-dev \
        libatlas-base-dev \
        && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# Use a non-priviledged user
RUN useradd -ms /bin/bash myuser
USER myuser

# Env variables
ENV PATH $PATH:/opt/conda/envs/env/bin:/opt/conda/bin:/home/myuser/.local/bin:/home/myuser/.local/lib
ENV CUDA_HOME "/usr/local/cuda-10.0"

WORKDIR /work

# Install requirements
RUN python3 -m pip install --user --upgrade pip
RUN python3 -m pip install --user Cython
COPY ./requirements.txt /work
RUN python3 -m pip install --user -r /work/requirements.txt

CMD ["/bin/bash"]