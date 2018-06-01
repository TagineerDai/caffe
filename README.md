# Caffe

Forked from [BLVC/caffe](https://github.com/BVLC/caffe).

### Installation

Based on `CUDA-9.0` with `cuDNN-7.0.5`.

```sh
# Install Pre-requisites
sudo apt-get install libprotobuf-dev libleveldb-dev libsnappy-dev libopencv-dev libhdf5-serial-dev protobuf-compiler
sudo apt-get install --no-install-recommends libboost-all-dev
sudo apt-get install libopenblas-dev liblapack-dev libatlas-base-dev
sudo apt-get install libgflags-dev libgoogle-glog-dev liblmdb-dev
sudo apt-get install python-numpy python-scipy
# Install & Make
git clone https://github.com/TagineerDai/caffe.git
cd caffe
cp Makefile.config.example Makefile.config
vim Makefile.config
# 6 is number of core, got by `echo $(($(nproc) + 1))`
sudo make all -j6
sudo make test -j6
sudo make pycaffe -j6
# Runtest should make without parallel, if there are any error, just repeated without make clean.
sudo make runtest 
```

