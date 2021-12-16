FROM ubuntu:20.04
#@sha256:88931f705e7c5a0ce04653f35c8b18cd2178f06f487daac4bbbd08497acfe97c

COPY ./mjkey.txt /opt/mujoco/mjkey.txt

ENV LC_ALL C
ENV SHELL /bin/bash

# For mujoco-py
ENV MUJOCO_PY_MUJOCO_PATH /opt/mujoco/mujoco200
ENV LD_LIBRARY_PATH $MUJOCO_PY_MUJOCO_PATH/bin:${LD_LIBRARY_PATH}
ENV MUJOCO_PY_MJKEY_PATH /opt/mujoco/mjkey.txt

# For DM control suites
ENV MJKEY_PATH $MUJOCO_PY_MJKEY_PATH
ENV MJLIB_PATH $MUJOCO_PY_MUJOCO_PATH/bin/libmujoco200.so

RUN apt update && apt install -y software-properties-common

RUN add-apt-repository universe

RUN apt update && DEBIAN_FRONTEND=noninteractive apt install -y \
        build-essential \
        git \
        wget \
        vim \
        emacs \
        curl \
        zip \
        net-tools \
        zlib1g-dev \
        unzip \
        pkg-config \
        libblas-dev \
        liblapack-dev \
        python3-tk \
        python3-pyqt5 \
        python3-wheel \
        python3-venv \
        python3-distutils \
        python3-cffi \
        python3.8 \
        python3.8-dev \
        pipenv \
        zsh \
        fish \
        cmake \
        libfluidsynth-dev \
        libfreetype6-dev \
        libgl1-mesa-dev \
        libgl1-mesa-glx \
        libglfw3 \
        libglew2.1 \
        libglew-dev \
        libglu1-mesa-dev \
        libgme-dev \
        libboost-all-dev \
        libosmesa6 \
        libosmesa6-dev \
        libpng-dev \
        libjpeg-dev \
        libsdl2-dev \
        libbz2-dev \
        libopenal-dev \
        xpra \
        xserver-xorg-dev \
        patchelf \
        ffmpeg \
        xvfb \
        spyder \
        timidity \
        tar  \
        swig \
        nasm

RUN apt-get clean && rm -rf /var/lib/apt/lists/*

# Setup Python3.8 and pip
RUN ln -s /usr/bin/python3.8 /usr/local/bin/python
RUN ln -s /usr/bin/python3.8 /usr/local/bin/python3
RUN curl -sL https://bootstrap.pypa.io/get-pip.py | python3.8

RUN unlink /run/shm


# Install some Python libraries
RUN python3.8 -m pip --no-cache-dir install \
        setuptools \
        wheel \
        cython \
        black \
        isort \
        flake8 \
        pytest \
        numpy \
        scipy \
        pandas==1.3.3 \
        scikit-learn \
        scikit-image \
        Pillow \
        opencv-python \
        matplotlib \
        seaborn \
        jupyterlab \
        jupyterlab_code_formatter \
        colorama \
        gym[atari]==0.19.0 \
        tqdm \
        plotly

# Install pybullet
RUN python3.8 -m pip install pybullet==3.0.8

# Install PyTorch
RUN python3.8 -m pip --no-cache-dir install \
        torch==1.7.1+cu110 \
        torchvision==0.8.2+cu110 \
        torchaudio==0.7.2 \
        -f https://download.pytorch.org/whl/torch_stable.html

# Install tensorflow
RUN python3.8 -m pip --no-cache-dir install \
        tensorflow==2.3.1 \
        tensorflow_probability==0.12.1

# Install some other RL related libraries
RUN python3.8 -m pip --no-cache-dir install \
        stable_baselines3==1.1.0\
        box2d-py==2.3.8 \
        gym[atari]==0.19.0 \
        nevergrad \
        ray \
        gym[classic_control]==0.19.0

# Install VizDoom environment
RUN python3.8 -m pip --no-cache-dir install \
        vizdoom \
        omgifol #for NavDoom

# Install MuJoCo
# We have to install mujoco-py at last, to ensure numpy binary compatibility
RUN wget https://www.roboti.us/download/mujoco200_linux.zip -O mujoco.zip \
        && unzip mujoco.zip \
        && mv mujoco200_linux $MUJOCO_PY_MUJOCO_PATH \
        && rm mujoco.zip

RUN python3.8 -m pip --no-cache-dir install dm_control
RUN python3.8 -m pip --no-cache-dir install git+git://github.com/denisyarats/dmc2gym.git
RUN python3.8 -m pip --no-cache-dir install git+git://github.com/kngwyu/mujoco-py.git@use-tmp-for-lock

# Remove the MuJoCo key
RUN rm $MUJOCO_PY_MJKEY_PATH