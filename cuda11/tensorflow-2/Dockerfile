FROM nvcr.io/nvidia/tensorflow:20.09-tf2-py3


# Install OpenSSH for MPI to communicate between containers
RUN  apt-get -o Acquire::Check-Valid-Until=false  -o Acquire::Check-Date=false   update && apt-get install -y --no-install-recommends openssh-client openssh-server && \
    mkdir -p /var/run/sshd

# python tools
RUN pip --no-cache-dir install jupyter \
                jupyterlab \
&& \
    rm -rf /root/.cache/pip/http/*

#
# Allow OpenSSH to talk to containers without asking for confirmation
RUN cat /etc/ssh/ssh_config | grep -v StrictHostKeyChecking > /etc/ssh/ssh_config.new && \
    echo "    StrictHostKeyChecking no" >> /etc/ssh/ssh_config.new && \
    cat /etc/ssh/sshd_config | grep -v  PermitRootLogin> /etc/ssh/sshd_config.new && \
    echo "PermitRootLogin yes" >> /etc/ssh/sshd_config.new && \
    mv /etc/ssh/ssh_config.new /etc/ssh/ssh_config && \
    mv /etc/ssh/sshd_config.new /etc/ssh/sshd_config

ENV JUPYTER_PORT 8888
EXPOSE 8888
##jupyter
RUN mkdir /etc/jupyter/ && wget -P /etc/jupyter/  https://raw.githubusercontent.com/Winowang/jupyter_gpu/master/jupyter_notebook_config.py
RUN wget -P /etc/jupyter/ https://raw.githubusercontent.com/Winowang/jupyter_gpu/master/custom.js
## tini
ENV TINI_VERSION v0.18.0
ADD https://github.com/krallin/tini/releases/download/${TINI_VERSION}/tini /tini
RUN chmod +x /tini

WORKDIR "/home"