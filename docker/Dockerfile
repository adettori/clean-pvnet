FROM pytorch/pytorch:1.4-cuda10.1-cudnn7-devel

# Hack to not have tzdata cmdline config during build
RUN ln -fs /usr/share/zoneinfo/Europe/Amsterdam /etc/localtime

# Keys for cuda repo
RUN gpg --keyserver keyserver.ubuntu.com --recv-keys A4B469963BF863CC
RUN gpg --export --armor A4B469963BF863CC | apt-key add

# Prepare apt
RUN apt update && \
    apt install -yq software-properties-common && \
    add-apt-repository --yes universe && \
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
        liblzma-dev \
        libbz2-dev \
        zlib1g-dev \
        xz-utils \
        libxml2-dev \
        libxmlsec1-dev \
        libzstd-dev

# Delete python2 package
RUN apt remove -yq python python-minimal python3.5 python3.5-minimal python3 python3-minimal

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
RUN ln -sf /usr/local/bin/python3.7 /usr/bin/python3
RUN ln -sf /usr/local/bin/python3.7 /usr/local/bin/python3
RUN ln -sf /usr/local/bin/python3.7 /usr/bin/python
RUN ln -sf /usr/local/bin/python3.7 /usr/local/bin/python

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
        libxcb-dri3-0 \
        libxcb-xinerama0 \
        libxcb1 \
        libspqr2 \
        && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# Hack to get uncertainty_pnp code library to work
RUN ln -s /usr/lib/x86_64-linux-gnu/libspqr.so.2.0.8 /usr/lib/x86_64-linux-gnu/libspqr.so.2.0.2
RUN ln -s /usr/lib/x86_64-linux-gnu/libcholmod.so.3.0.11 /usr/lib/x86_64-linux-gnu/libcholmod.so.3.0.6
RUN ln -s /usr/lib/x86_64-linux-gnu/libcxsparse.so.3.2.0 /usr/lib/x86_64-linux-gnu/libcxsparse.so.3.1.4

# Remove conda
RUN rm -r /opt/conda

# Force bash as main shell
RUN ln -sf /bin/bash /bin/sh

# Use a non-priviledged user
RUN useradd -ms /bin/bash myuser
USER myuser

# Env variables
ENV PATH $PATH:/home/myuser/.local/bin:/home/myuser/.local/lib
ENV CUDA_HOME "/usr/local/cuda-10.1"

WORKDIR /work

# Install requirements
RUN python3 -m venv pvnet-env
RUN source pvnet-env/bin/activate && python3 -m pip install --upgrade pip
RUN source pvnet-env/bin/activate && python3 -m pip install Cython==0.28.2 setuptools==57.5.0 wheel==0.42.0
RUN source pvnet-env/bin/activate && python3 -m pip install --index-url https://download.pytorch.org/whl/cu101/ torch==1.3.0
COPY ./requirements.txt /work
RUN source pvnet-env/bin/activate && python3 -m pip install -r /work/requirements.txt    

CMD ["/bin/bash"]