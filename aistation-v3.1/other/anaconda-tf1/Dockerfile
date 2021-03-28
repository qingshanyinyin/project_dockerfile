FROM nvcr.io/nvidia/tensorflow:19.01-py3

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

# python tools
RUN pip --no-cache-dir install \
                scipy \
                opencv-python \
                tensorboardX \
                mxboard  \
                tqdm \
                mpi4py \
                matplotlib \
                visdom \
                keras==2.2.4 \
    && rm -rf /root/.cache/pip/http/*

#jupyter
RUN mkdir /etc/jupyter/ && wget -P /etc/jupyter/  https://raw.githubusercontent.com/Winowang/jupyter_gpu/master/jupyter_notebook_config.py \
    &&  wget -P /etc/jupyter/ https://raw.githubusercontent.com/Winowang/jupyter_gpu/master/custom.js \
#anaconda
    && mkdir /tmp/anaconda/ && wget -P /tmp/anaconda/ https://repo.anaconda.com/archive/Anaconda3-5.2.0-Linux-x86_64.sh \
    && bash /tmp/anaconda/Anaconda3-5.2.0-Linux-x86_64.sh -b -p /home/anaconda3 && rm -rf /tmp/anaconda/*
ENV PATH /home/anaconda3/bin:$PATH

## tini
ENV TINI_VERSION v0.18.0
ADD https://github.com/krallin/tini/releases/download/${TINI_VERSION}/tini /tini
RUN chmod +x /tini

WORKDIR "/home"
