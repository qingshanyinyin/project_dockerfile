FROM qingshanyinyin2000/deeplearning:anaconda3

# conda install cuda and so on.
# RUN source /home/anaconda3/etc/profile.d/conda.sh
RUN conda create -n tensorflow python=3.6
RUN conda activate tensorflow
RUN conda install cudatoolkit=10.0 && conda install nccl && conda install cudnn && \
    conda install tensorflow-gpu==1.12 && conda install keras=2.2.4
#RUN source deactivate
WORKDIR "/home"
