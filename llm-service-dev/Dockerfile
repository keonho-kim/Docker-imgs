FROM ubuntu:22.04

ENV LANG=C.UTF-8 LC_ALL=C.UTF-8
ENV PATH /opt/conda/bin:$PATH

WORKDIR /home

COPY requirements.txt /home/requirements.txt

RUN set -x && \
    apt-get update --fix-missing && \
	DEBIAN_FORNTEND=noninteractive TZ=Etc/UTC apt-get install -y tzdata && \
    apt-get install -y --no-install-recommends \
		sudo \
        software-properties-common \
        bzip2 \
        ca-certificates \
        cmake \
        build-essential \
        git \
        curl \
        gnupg \
        gcc \
        g++ \
        libomp-dev \
        libglib2.0-0 \
        libsm6 \
        libxcomposite1 \
        libxcursor1 \
        libxdamage1 \
        libxext6 \
        libxfixes3 \
        libxi6 \
        libxinerama1 \
        libxrandr2 \
        libxrender1 \
        mercurial \
        openssh-client \
        procps \
        subversion \
        wget \
        ffmpeg \
        apt-file \
        vim \
    && apt-file update \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* && \
    UNAME_M="$(uname -m)" && \
    if [ "${UNAME_M}" = "x86_64" ]; then \
        ANACONDA_URL="https://repo.anaconda.com/archive/Anaconda3-2023.09-0-Linux-x86_64.sh"; \
        SHA256SUM="6c8a4abb36fbb711dc055b7049a23bbfd61d356de9468b41c5140f8a11abd851"; \
    elif [ "${UNAME_M}" = "s390x" ]; then \
        ANACONDA_URL="https://repo.anaconda.com/archive/Anaconda3-2023.09-0-Linux-s390x.sh"; \
        SHA256SUM="ee817071a2ad94e044fb48061a721bc86606b2f4906b705e4f42177eeb3ca7c5"; \
    elif [ "${UNAME_M}" = "aarch64" ]; then \
        ANACONDA_URL="https://repo.anaconda.com/archive/Anaconda3-2023.09-0-Linux-aarch64.sh"; \
        SHA256SUM="69ee26361c1ec974199bce5c0369e3e9a71541de7979d2b9cfa4af556d1ae0ea"; \
    elif [ "${UNAME_M}" = "ppc64le" ]; then \
        ANACONDA_URL="https://repo.anaconda.com/archive/Anaconda3-2023.09-0-Linux-ppc64le.sh"; \
        SHA256SUM="5ea1ed9808af95eb2655fe6a4ffdb66bea66ecd1d053fc2ee69eacc7685ef665"; \
    fi && \
    wget "${ANACONDA_URL}" -O anaconda.sh -q && \
    echo "${SHA256SUM} anaconda.sh" > shasum && \
    sha256sum --check --status shasum && \
    /bin/bash anaconda.sh -b -p /opt/conda && \
    rm anaconda.sh shasum && \
    ln -s /opt/conda/etc/profile.d/conda.sh /etc/profile.d/conda.sh && \
    echo ". /opt/conda/etc/profile.d/conda.sh" >> ~/.bashrc && \
    echo "conda activate base" >> ~/.bashrc && \
    find /opt/conda/ -follow -type f -name '*.a' -delete && \
    find /opt/conda/ -follow -type f -name '*.js.map' -delete && \
    /opt/conda/bin/conda clean -afy

# Whisper.cpp with models
RUN git clone https://github.com/ggerganov/whisper.cpp.git && \
	cd ./whisper.cpp && make

# NodeJS
RUN sudo apt-get update && sudo apt-get install nodejs -y

# SQLite 3 & MongoDB
RUN sudo apt install sqlite3 -y && \
    sudo add-apt-repository universe && \
    wget -qO - https://www.mongodb.org/static/pgp/server-7.0.asc | \
    gpg --dearmor | \
    sudo tee /usr/share/keyrings/mongodb.gpg > /dev/null && \
    echo "deb [ arch=amd64,arm64 signed-by=/usr/share/keyrings/mongodb.gpg ] https://repo.mongodb.org/apt/ubuntu jammy/mongodb-org/7.0 multiverse" | \
    sudo tee /etc/apt/sources.list.d/mongodb-org-7.0.list && \
    sudo apt-get update && \
    sudo apt-get install -y mongodb-org && \
    echo "mongodb-org hold" | sudo dpkg --set-selections && \
    echo "mongodb-org-database hold" | sudo dpkg --set-selections && \
    echo "mongodb-org-server hold" | sudo dpkg --set-selections && \
    echo "mongodb-mongosh hold" | sudo dpkg --set-selections && \
    echo "mongodb-org-mongos hold" | sudo dpkg --set-selections && \
    echo "mongodb-org-tools hold" | sudo dpkg --set-selections

# Apache Spark
RUN sudo apt install openjdk-17-jdk -y && \
    wget https://dlcdn.apache.org/spark/spark-3.5.0/spark-3.5.0-bin-hadoop3.tgz && \
    tar xvf spark-3.5.0-bin-hadoop3.tgz && \
    sudo mv spark-3.5.0-bin-hadoop3 /opt/spark && \
    echo "export SPARK_HOME=/opt/spark" >> ~/.bashrc && \
    echo "export PATH=$PATH:$SPARK_HOME/bin" >> ~/.bashrc && \
    echo "source ~/.bashrc"

# LLaMA.cpp
RUN git clone https://github.com/ggerganov/llama.cpp && \
	cd llama.cpp && make

# Dev Requirements
RUN pip install -U -r /home/requirements.txt && rm /home/requirements.txt

CMD [ "/bin/bash" ]
CMD ["jupyter", "lab", "--port=8888", "--ip=0.0.0.0", "--no-browser", "--allow-root"]
