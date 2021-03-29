FROM ubuntu:18.04

# common tools
RUN apt-get -o Acquire::Check-Valid-Until=false  -o Acquire::Check-Date=false   update && apt-get install -y --allow-downgrades --allow-change-held-packages --no-install-recommends \
        curl \
        vim \
        wget \
        net-tools \
        inetutils-ping \
        bzip2 \
        unzip
#anaconda
RUN mkdir /tmp/anaconda/ && wget --no-check-certificate -P /tmp/anaconda/ https://repo.anaconda.com/archive/Anaconda3-5.2.0-Linux-x86_64.sh \
    && bash /tmp/anaconda/Anaconda3-5.2.0-Linux-x86_64.sh -b -p /usr/local/anaconda3 && rm -rf /tmp/anaconda/*
ENV PATH /usr/local/anaconda3/bin:$PATH

RUN conda install python=3.6
#conda
RUN conda install cudatoolkit=10.0
RUN conda install nccl
RUN conda install cudnn
RUN conda install tensorflow-gpu==1.12
RUN conda install keras=2.2.4

WORKDIR "/home"
