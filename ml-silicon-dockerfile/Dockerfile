FROM ubuntu:20.04

ENV LANG=C.UTF-8 LC_ALL=C.UTF-8
ENV PATH /opt/conda/bin:$PATH

WORKDIR /home

COPY requirements.txt /home/requirements.txt

RUN set -x && \
    apt-get update --fix-missing && \
	DEBIAN_FORNTEND=noninteractive TZ=Etc/UTC apt-get install -y tzdata && \
    apt-get install -y --no-install-recommends \
		sudo \
        bzip2 \
        ca-certificates \
        cmake \
        build-essential \
        git \
        curl \
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

RUN pip install -r /home/requirements.txt && rm /home/requirements.txt

# INSTALL LIGHTGBM
RUN conda config --set always_yes yes --set changeps1 no && \
    git clone --recursive --branch stable --depth 1 https://github.com/Microsoft/LightGBM && \
    cd ./LightGBM && \
    sh ./build-python.sh install

RUN pip uninstall graphviz -y && \
    pip install dtreeviz dtreeviz[xgboost] dtreeviz[lightgbm]

RUN apt-get update && \
	apt-get install apt-file -y && \
	apt-file update && \
	apt-get install vim -y

# REINSTALL SCIPY & NUMPY FOR DEPENDENCY ISSUE
RUN pip install --force-reinstall scipy~=1.10.1 numpy==1.24.2

CMD [ "/bin/bash" ]
CMD ["jupyter", "lab", "--port=8888", "--ip=0.0.0.0", "--no-browser", "--allow-root"]
