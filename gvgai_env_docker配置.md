## Dockerfile for GVGAI environment

### Requirements

- [JAVA](https://www.oracle.com/technetwork/java/javase/downloads/java-archive-javase9-3934878.html)
- [Anaconda](https://www.anaconda.com/distribution/)    [中文镜像（清华）](https://mirrors.tuna.tsinghua.edu.cn/anaconda/archive/)
- [GVGAI](https://github.com/rubenrtorrado/GVGAI_GYM) : A framework for game AI

### Dockerfile

```dockerfile
FROM ubuntu:16.04

# install java manually
ADD jdk-9.tar.gz /usr/local

WORKDIR /package
COPY anaconda.sh /package
ADD GVGAI_GYM/ /package/GVGAI_GYM

# install anaconda
RUN apt-get update && apt-get install -y \
    vim && \
    bash anaconda.sh -b -p /opt/conda && \
    rm -rf anaconda.sh

# set up JAVA environment
ENV JAVA_HOME=/usr/local/jdk-9.0.4
ENV CLASSPATH=.:$JAVA_HOME/lib:$CLASSPATH
ENV PATH=$JAVA_HOME/bin:$PATH

# set up python environment
ENV PATH=/opt/conda/bin:$PATH

# install GVGAI_GIM, tensorflow, pytorch
RUN pip install -e GVGAI_GYM/
RUN pip install --index-url https://pypi.douban.com/simple tensorflow && \
    pip install torch==1.3.0+cpu torchvision==0.4.1+cpu -f https://download.pytorch.org/whl/torch_stable.html
```

### Folder 

```
└─ GVGAI_GYM/
└─ anaconda.sh
└─ jdk-9.0.4_linux-x64_bin.tar.gz
└─Dockerfile
```

