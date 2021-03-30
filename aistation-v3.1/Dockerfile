FROM nvidia/cuda:9.0-cudnn7-devel-ubuntu16.04
MAINTAINER lxy <lxy@163.com>

# install basic dependencies
RUN apt-get update
RUN apt-get install -y wget \
                vim \
                cmake

# install Anaconda3
RUN wget https://mirrors.tuna.tsinghua.edu.cn/anaconda/archive/Anaconda3-5.2.0-Linux-x86_64.sh -O ~/anaconda3.sh
RUN bash ~/anaconda3.sh -b -p /home/anaconda3 \
        && rm ~/anaconda3.sh
ENV PATH /home/anaconda3/bin:$PATH

# change mirror
RUN mkdir ~/.pip \
        && cd ~/.pip
RUN     echo -e "[global]\nindex-url = https://pypi.mirrors.ustc.edu.cn/simple/" >> ~/pip.conf

# install tensorflow
RUN /home/anaconda3/bin/pip install tensorflow-gpu==1.12.0

RUN /home/anaconda3/bin/pip install keras==2.2.4 && rm -rf /root/.cache/pip/http/*

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

# jupyter
RUN mkdir /etc/jupyter/ && wget -P /etc/jupyter/  https://raw.githubusercontent.com/Winowang/jupyter_gpu/master/jupyter_notebook_config.py \
    &&  wget -P /etc/jupyter/ https://raw.githubusercontent.com/Winowang/jupyter_gpu/master/custom.js \

## tini
ENV TINI_VERSION v0.18.0
ADD https://github.com/krallin/tini/releases/download/${TINI_VERSION}/tini /tini
RUN chmod +x /tini

WORKDIR "/home"
