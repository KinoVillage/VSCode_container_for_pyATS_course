FROM ubuntu:18.04
USER root
SHELL ["/bin/bash", "-l", "-c"]

ARG USERNAME=vscode
ARG USER_UID=1000
ARG USER_GID=$USER_UID

RUN apt-get update && \
    apt-get -y install --no-install-recommends apt-utils dialog 2>&1 && \
    apt-get -y install sudo wget vim less git locales python3-pip && \
    localedef -f UTF-8 -i ja_JP ja_JP.UTF-8

RUN groupadd --gid $USER_GID $USERNAME && \
    useradd -s /bin/bash --uid $USER_UID --gid $USER_GID -m $USERNAME && \
    echo $USERNAME ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USERNAME && \
    chmod 0440 /etc/sudoers.d/$USERNAME

ENV LANG ja_JP.UTF-8
ENV LANGUAGE ja_JP:ja
ENV LC_ALL ja_JP.UTF-8
ENV TZ JST-9
ENV TERM xterm

RUN apt-get update && \
    apt-get -y upgrade && \
    apt-get install libgl1-mesa-glx libegl1-mesa libxrandr2 libxrandr2 libxss1 \
                    libxcursor1 libxcomposite1 libasound2 libxi6 libxtst6 -y && \
    wget -P /opt https://repo.anaconda.com/archive/Anaconda3-2020.02-Linux-x86_64.sh && \
    bash /opt/Anaconda3-2020.02-Linux-x86_64.sh -b -p /opt/anaconda3 && \
    rm /opt/Anaconda3-2020.02-Linux-x86_64.sh

RUN source ~/.bashrc
ENV PATH /opt/anaconda3/bin:$PATH

WORKDIR /code
ADD requirements.txt /code
RUN echo $PATH && \
    conda create -n kino_net python=3.6 && \
    source activate kino_net && \
    pip3 install --upgrade pip && \
    pip3 install --upgrade setuptools && \
    pip3 install -r requirements.txt