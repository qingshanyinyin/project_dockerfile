FROM continuumio/anaconda3:2020.11

# common tools
RUN apt-get -o Acquire::Check-Valid-Until=false  -o Acquire::Check-Date=false   update && apt-get install -y --allow-downgrades --allow-change-held-packages --no-install-recommends \
        #build-essential \
        #cmake \
        #software-properties-common \
        #git \
        #curl \
        #vim \
        #wget \
        net-tools \
        inetutils-ping \
        bzip2 \
        unzip

#conda
RUN conda install cudatoolkit=10.0 && conda install nccl && conda install cudnn && \
    conda install python=3.6 && conda install tensorflow-gpu==1.12 && conda install keras=2.2.4

# Install OpenSSH for MPI to communicate between containers
RUN  apt-get -o Acquire::Check-Valid-Until=false  -o Acquire::Check-Date=false   update && apt-get install -y --no-install-recommends openssh-client openssh-server && \
    mkdir -p /var/run/sshd && \
   apt-get clean && rm -rf /var/lib/apt/lists/* && rm -rf /root/.cache/pip/http/*

# Allow OpenSSH to talk to containers without asking for confirmation
RUN cat /etc/ssh/ssh_config | grep -v StrictHostKeyChecking > /etc/ssh/ssh_config.new && \
    echo "    StrictHostKeyChecking no" >> /etc/ssh/ssh_config.new && \
    cat /etc/ssh/sshd_config | grep -v  PermitRootLogin> /etc/ssh/sshd_config.new && \
    echo "PermitRootLogin yes" >> /etc/ssh/sshd_config.new && \
    mv /etc/ssh/ssh_config.new /etc/ssh/ssh_config && \
    mv /etc/ssh/sshd_config.new /etc/ssh/sshd_config

#jupyter
RUN rm -rf /opt/conda/etc/jupyter/* && \
    wget -P /opt/conda/etc/jupyter/  https://raw.githubusercontent.com/Winowang/jupyter_gpu/master/jupyter_notebook_config.py && \
    wget -P /opt/conda/etc/jupyter/  https://raw.githubusercontent.com/Winowang/jupyter_gpu/master/custom.js

# Install Open MPI
RUN mkdir /tmp/openmpi && \
    cd /tmp/openmpi && \
    wget https://www.open-mpi.org/software/ompi/v3.1/downloads/openmpi-3.1.3.tar.gz && \
    tar zxf openmpi-3.1.3.tar.gz && \
    cd openmpi-3.1.3 && \
    ./configure --enable-orterun-prefix-by-default  --with-cuda=/usr/local/cuda  && \
    make -j $(nproc) all && \
    make install && \
    ldconfig && \
    rm -rf /tmp/openmpi

# Install Horovod, temporarily using CUDA stubs
RUN ldconfig /usr/local/cuda/targets/x86_64-linux/lib/stubs && \
    HOROVOD_GPU_ALLREDUCE=NCCL  HOROVOD_WITH_TENSORFLOW=1 pip install --no-cache-dir horovod==0.15.1 && \
    ldconfig

## tini
ENV TINI_VERSION v0.18.0
ADD https://github.com/krallin/tini/releases/download/${TINI_VERSION}/tini /tini
RUN chmod +x /tini

WORKDIR "/home"
