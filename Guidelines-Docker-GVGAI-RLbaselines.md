## Guidelines-Docker-GVGAI-RLbaselines.md

### CPU version

**1. Installing docker**

**2. Download required package**

- Anaconda   [link](https://www.anaconda.com/distribution/)  [清华镜像](https://mirrors.tuna.tsinghua.edu.cn/anaconda/archive/)
- Java [link](https://www.oracle.com/technetwork/java/javase/downloads/java-archive-javase9-3934878.html)

**3. Dockerfile**  (modified from the rl_baseline repository)

```dockerfile
FROM ubuntu:16.04

# Download jdk-9 from  https://www.oracle.com/technetwork/java/javase/downloads/java-archive-javase9-3934878.html
ADD jdk-9.tar.gz /usr/local

RUN apt-get -y update && apt-get -y install vim git libopenmpi-dev zlib1g-dev cmake libglib2.0-0 libsm6 libxext6 libfontconfig1 libxrender1

WORKDIR /package

# Download Anaconda from https://www.anaconda.com/distribution/ 
COPY anaconda.sh /package
RUN bash anaconda.sh -b -p /opt/conda && \
    rm -rf anaconda.sh


ENV JAVA_HOME=/usr/local/jdk-9.0.4
ENV CLASSPATH=.:$JAVA_HOME/lib:$CLASSPATH
ENV PATH=$JAVA_HOME/bin:$PATH
ENV PATH=/opt/conda/bin:$PATH

RUN git clone https://github.com/rubenrtorrado/GVGAI_GYM.git

# The rl-baseline required ternsorflow version to be from 1.8.0 to 1.14.0
RUN pip install -e GVGAI_GYM/ && \
    pip install codacy-coverage && \
    pip install mpi4py && \
    pip install tensorflow==1.14.0 && \
    pip install scipy==1.1.0 && \
    pip install cloudpickle && \
    pip install stable-baselines[mpi]

# Install pytorch cpu version by uncommenting the following line   
# RUN pip install torch==1.3.0+cpu torchvision==0.4.1+cpu -f https://download.pytorch.org/whl/torch_stable.html

CMD /bin/bash
```

*PS: anaconda.sh jdk.tar.gz and Dockerfile should be in the same folder*!

### GPU version

If you have GPU in your machine, you can use GPU to speed up the learning process. 

**1. Installing NAVIDA driver in your machine**

**2. Installing the docker in your machie**

**3. Installing NAVIDA docker** (It is simply a plugin to Docker to support GPU using in container)

*(PS: The above three step can be refer to the existing blog. [link](https://chunml.github.io/ChunML.github.io/project/Installing-NVIDIA-Docker-On-Ubuntu-16.04/))*

**4. Use GPU in docker**

- In the dockerfile, use `FROM nvidia/cuda:9.0-cudnn7-runtime-ubuntu16.04` instead of `From Ubuntu:16.04`. 

- The version of TensorFlow and Pytorch should be GPU version.

- When run container from image, add `--runtime=nvdia ` in the command. 

  E.g. `docker run --runtime=nvidia -v $PWD:/home --rm -it baseline_gpu /bin/bash`

- Use `nvidia-smi` to find out the status of GPU. 



```dockerfile
FROM nvidia/cuda:9.0-cudnn7-runtime-ubuntu16.04

# Download jdk-9 from  https://www.oracle.com/technetwork/java/javase/downloads/java-archive-javase9-3934878.html
ADD jdk-9.tar.gz /usr/local

RUN apt-get -y update && apt-get -y install vim git libopenmpi-dev zlib1g-dev cmake libglib2.0-0 libsm6 libxext6 libfontconfig1 libxrender1

WORKDIR /package

# Download Anaconda from https://www.anaconda.com/distribution/ 
COPY anaconda.sh /package
RUN bash anaconda.sh -b -p /opt/conda && \
    rm -rf anaconda.sh


ENV JAVA_HOME=/usr/local/jdk-9.0.4
ENV CLASSPATH=.:$JAVA_HOME/lib:$CLASSPATH
ENV PATH=$JAVA_HOME/bin:$PATH
ENV PATH=/opt/conda/bin:$PATH

RUN git clone https://github.com/rubenrtorrado/GVGAI_GYM.git

# The rl-baseline required ternsorflow version to be from 1.8.0 to 1.14.0
RUN pip install -e GVGAI_GYM/ && \
    pip install codacy-coverage && \
    pip install mpi4py && \
    pip install tensorflow-gpu==1.14 && \
    pip install scipy==1.1.0 && \
    pip install cloudpickle && \
    pip install stable-baselines[mpi]

# Install pytorch cpu version by uncommenting the following line   
# RUN conda install pytorch==1.1.0 torchvision==0.3.0 cudatoolkit==9.0 -c pytorch

CMD /bin/bash
```



### Reference

1. [G. Brockman, V. Cheung, L. Pettersson, J. Schneider, J. Schulman, J. Tang, and W. Zaremba, “Openai gym,” 2016.](https://github.com/openai/gym)
2. [A. Hill, A. Raffin, M. Ernestus, A. Gleave, A. Kanervisto, R. Traore, P. Dhariwal, C. Hesse, O. Klimov, A. Nichol, M. Plappert, A. Radford, J. Schulman, S. Sidor, and Y. Wu, “Stable baselines,” https://github.com/hill-a/stable-baselines, 2018.](https://github.com/hill-a/stable-baselines)
3. [R. R. Torrado, P. Bontrager, J. Togelius, J. Liu, and D. Perez-Liebana, “Deep reinforcement learning for general video game ai,” in Computational Intelligence and Games (CIG), 2018 IEEE Conference on. IEEE,
   2018.](https://github.com/rubenrtorrado/GVGAI_GYM)





