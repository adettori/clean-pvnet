FROM yangfei4/pvnet_clean:non-ros

# Hack to not have tzdata cmdline config during build
RUN ln -fs /usr/share/zoneinfo/Europe/Amsterdam /etc/localtime

# Install python3.7 and dependencies, taken from:
# - hhttps://websiteforstudents.com/installing-the-latest-python-3-7-on-ubuntu-16-04-18-04/
# - https://github.com/zju3dv/pvnet/blob/master/docker/Dockerfile
# - https://github.com/zju3dv/clean-pvnet
RUN apt-get update && \
    apt install -yq software-properties-common && \
    add-apt-repository ppa:deadsnakes/ppa && \
    apt update && \
    apt install -yq \
        nano \
        sudo \
        wget \
        curl \
        build-essential \
        cmake \
        git \
        ca-certificates \
        python3.7 \
        python3-pip \
        python-qt4 \
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

WORKDIR /work

COPY ./requirements.txt /work

# installing PVnet dependencies
RUN pip install --user torch==1.1.0 -f https://download.pytorch.org/whl/cu90/stable && \
    pip install --user Cython==0.28.2 && \
    pip install --user -r /work/requirements.txt && \
    pip install --user transforms3d

CMD ["/bin/bash"]