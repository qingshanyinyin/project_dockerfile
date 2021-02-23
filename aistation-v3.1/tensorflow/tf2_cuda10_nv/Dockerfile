FROM nvcr.io/nvidia/tensorflow:20.03-tf2-py3

# Install OpenSSH for MPI to communicate between containers
RUN  apt-get -o Acquire::Check-Valid-Until=false  -o Acquire::Check-Date=false   update && apt-get install -y --no-install-recommends openssh-client openssh-server && \
    mkdir -p /var/run/sshd

# python tools

#
# Allow OpenSSH to talk to conta    iners without asking for confirmation
RUN cat /etc/ssh/ssh_config | grep -v StrictHostKeyChecking > /etc/ssh/ssh_config.new && \
    echo "    StrictHostKeyChecking no" >> /etc/ssh/ssh_config.new && \
    cat /etc/ssh/sshd_config | grep -v  PermitRootLogin> /etc/ssh/sshd_config.new && \
    echo "PermitRootLogin yes" >> /etc/ssh/sshd_config.new && \
    mv /etc/ssh/ssh_config.new /etc/ssh/ssh_config && \
    mv /etc/ssh/sshd_config.new /etc/ssh/sshd_config

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
                keras==2.3.1 \
                -U scikit-learn \
&& rm -rf /root/.cache/pip/http/*

##jupyter
RUN rm -rf /usr/local/etc/jupyter/jupyter_notebook_config.py && \
	rm -rf /usr/local/etc/jupyter/custom.js
RUN wget -P /usr/local/etc/jupyter/ https://raw.githubusercontent.com/Winowang/jupyter_gpu/master/jupyter_notebook_config.py
RUN wget -P /usr/local/etc/jupyter/ https://raw.githubusercontent.com/Winowang/jupyter_gpu/master/custom.js
## tini
ENV TINI_VERSION v0.18.0
ADD https://github.com/krallin/tini/releases/download/${TINI_VERSION}/tini /tini
RUN chmod +x /tini

WORKDIR "/home"
