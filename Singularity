BootStrap: debootstrap
OSVersion: xenial
MirrorURL: http://us.archive.ubuntu.com/ubuntu/


%runscript
    echo "SINGULARITY RUNSCRIPT"
    echo "Arguments received: $*"
    /usr/bin/python3
%post
    echo "Hello from inside the container"

    export LC_ALL=C
    export PATH=/bin:/sbin:/usr/bin:/usr/sbin:$PATH

    # add universe repo and install some packages
    # sed -i '/xenial.*universe/s/^#//g' /etc/apt/sources.list
    
    apt-get install -y software-properties-common
    add-apt-repository universe
    apt-get -y update
    apt-get -y install locales
    locale-gen en_US.UTF-8
    apt-get -y install python3-setuptools

    apt-get install -y \
    wget \
    cmake \
    git \
    libeigen3-dev \
    libcgal-dev \
    libgmp-dev \
    libgmpxx4ldbl \
    libsuitesparse-dev  \
    libcgal-dev \
    python3-nose \
    python3-numpy \
    python3-scipy \
    python3-setuptools \
    python3-dev \
    swig \
    && apt-get clean
    apt-get clean
    

    mkdir /om || echo "/OM exists"
    mkdir /cm || echo "/CM exists"

    cd /
    git clone https://github.com/qnzhou/PyMesh.git
    cd PyMesh && git submodule init && git submodule update
    export PYMESH_PATH=/PyMesh
    mkdir -p $PYMESH_PATH/third_party/build && mkdir -p $PYMESH_PATH/build
    cd $PYMESH_PATH/third_party/build

    echo "Building third party tools"
    cmake .. && make && make install && make clean

    echo "Building PyMesh and running tests"
    cd $PYMESH_PATH/build
    cmake .. && make && make tools && make all_tests && make clean

    echo "Installing PyMesh"
    cd $PYMESH_PATH
    python3 setup.py build
    python3 setup.py install


%test

	python3 -c "import pymesh; pymesh.test()"