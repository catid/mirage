# Mirage Installation

The quickest way to try Mirage is through our prebuilt docker images with all dependencies preinstalled. Mirage can also be built from source code using the following instructions.

## Docker images

We require [docker](https://docs.docker.com/engine/installation/) and [NVIDIA Container Toolkit](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html).

* First, clone the Mirage gitpub repository to obtain necessary scripts.
```bash
git clone --recursive https://www.github.com/mirage-project/mirage
cd mirage/docker

# Build docker image
docker build -t mirage -f Dockerfile .

# Run docker image
docker run --gpus all -it mirage bash

# Run test script
python demo/demo_group_query_attention_spec_decode.py --checkpoint demo/checkpoint_group_query_attn_spec_decode.json
```

## Install from source code

### Prerequisties

* CMAKE 3.24 or higher
* Cython 0.28 or higher
* CUDA 11.0 or higher and CUDNN 8.0 or higher

```bash
sudo apt-get install cmake make cython
sudo apt-get install libcudnn8-dev
sudo apt-get install libcudnn8=8.0.4.30-1+cuda11.0
```

### Clone github repo

To get started, you can clone the Mirage source code from github.
```bash
git clone --recursive https://www.github.com/mirage-project/mirage
cd mirage
```

### Install Z3 or build Z3 from source code

If the environment does not have a pre-installed Z3, you can build Z3 from source code using the following command lines
```bash
cd /path-to-mirage/deps/z3
mkdir build
cd build
cmake ..
make -j
export LD_LIBRARY_PATH=/path-to-mirage/deps/z3/build:LD_LIBRARY_PATH
```
This will install Z3 in the `/path-to-mirage/deps/z3/build` folder and add the fold into `LD_LIBRARY_PATH`.

### Build the Mirage runtime library
Second, you will need to build the Mirage runtime library. You will need to set `CUDACXX` and `Z3_DIR` to let cmake find the paths to CUDA and Z3 librarires.
```bash
export CUDACXX=/usr/local/cuda/bin/nvcc
export Z3_DIR=/path-to-mirage/deps/z3/build
mkdir build; cd build; cmake ..
make -j
```

### Install the Mirage python package
Finally, you will install the Mirage python package, which allows you to use Mirage's python package to superoptimize DNNs.
```bash
cd /path-to-mirage/python
python setup.py install
```
