# Official `BLAS/caffe:master` Installation
For DEMO [FCN@ucb](https://github.com/shelhamer/fcn.berkeleyvision.org).

prereq  
```
$ sudo apt-get install libprotobuf-dev libleveldb-dev libsnappy-dev libopencv-dev libhdf5-serial-dev protobuf-compiler
$ sudo apt-get install --no-install-recommends libboost-all-dev
```
make  
```
$ cp Makefile.config.example Makefile.config
$ vim Makefile.config
...
$ make -j< number of cores * 2 >
```

```
CXX src/caffe/solvers/rmsprop_solver.cpp
In file included from ./include/caffe/blob.hpp:10:0,
                 from ./include/caffe/net.hpp:10,
                 from ./include/caffe/solver.hpp:7,
                 from ./include/caffe/sgd_solvers.hpp:7,
                 from src/caffe/solvers/rmsprop_solver.cpp:3:
./include/caffe/syncedmem.hpp:7:19: fatal error: mkl.h: No such file or directory
compilation terminated.
Makefile:581: recipe for target '.build_release/src/caffe/solvers/rmsprop_solver.o' failed
make: *** [.build_release/src/caffe/solvers/rmsprop_solver.o] Error 1

```

Install ATLAS or OpenBLAS  
ATLAS for **with CUDA installation**, OpenBLAS for **CPU only installation**.  
```
sudo apt-get install libatlas-base-dev
sudo apt-get install libopenblas-base
```

```
In file included from src/caffe/solvers/sgd_solver.cpp:5:0:
./include/caffe/util/hdf5.hpp:6:18: fatal error: hdf5.h: No such file or directory
compilation terminated.
Makefile:581: recipe for target '.build_release/src/caffe/solvers/sgd_solver.o' failed
make: *** [.build_release/src/caffe/solvers/sgd_solver.o] Error 1

```  

Change `NCLUDE_DIRS := $(PYTHON_INCLUDE) /usr/local/include` to `INCLUDE_DIRS := $(PYTHON_INCLUDE) /usr/local/include /usr/include/hdf5/serial/` in `Makefile.config` accroding to [this page](https://github.com/BVLC/caffe/issues/2347).

```
CXX src/caffe/util/upgrade_proto.cpp
In file included from src/caffe/util/db.cpp:3:0:
./include/caffe/util/db_lmdb.hpp:8:18: fatal error: lmdb.h: No such file or directory
compilation terminated.
Makefile:581: recipe for target '.build_release/src/caffe/util/db.o' failed
make: *** [.build_release/src/caffe/util/db.o] Error 1

```

Install LMDB  
```
$ sudo apt-get install liblmdb-dev
```  

```
LD -o .build_release/lib/libcaffe.so.1.0.0
/usr/bin/ld: cannot find -lgflags
/usr/bin/ld: cannot find -lhdf5_hl
/usr/bin/ld: cannot find -lhdf5
collect2: error: ld returned 1 exit status
Makefile:572: recipe for target '.build_release/lib/libcaffe.so.1.0.0' failed
make: *** [.build_release/lib/libcaffe.so.1.0.0] Error 1
```

```
$ sudo apt-get install libgflags-dev
```
Change `LIBRARY_DIRS := $(PYTHON_LIB) /usr/local/lib /usr/lib` to `LIBRARY_DIRS := $(PYTHON_LIB) /usr/local/lib /usr/lib /usr/lib/x86_64-linux-gnu/hdf5/serial/` in `Makefile.config` accroding to [this page](https://github.com/BVLC/caffe/issues/4333).  

```
CXX/LD -o .build_release/tools/upgrade_net_proto_binary.bin
.build_release/lib/libcaffe.so: undefined reference to `cv::imread(cv::String const&, int)'
.build_release/lib/libcaffe.so: undefined reference to `cv::imencode(cv::String const&, cv::_InputArray const&, std::vector<unsigned char, std::allocator<unsigned char> >&, std::vector<int, std::allocator<int> > const&)'
.build_release/lib/libcaffe.so: undefined reference to `cv::imdecode(cv::_InputArray const&, int)'
collect2: error: ld returned 1 exit status
Makefile:625: recipe for target '.build_release/tools/upgrade_net_proto_binary.bin' failed
make: *** [.build_release/tools/upgrade_net_proto_binary.bin] Error 1
```

change OpenCV version to 3 accroding to [this page](https://github.com/BVLC/caffe/issues/3700).

```
CXX/LD -o .build_release/tools/compute_image_mean.bin
.build_release/tools/compute_image_mean.o: In function `_GLOBAL__sub_I_compute_image_mean.cpp':
compute_image_mean.cpp:(.text.startup+0x16d): undefined reference to `google::FlagRegisterer::FlagRegisterer<std::__cxx11::basic_string<char, std::char_traits<char>, std::allocator<char> > >(char const*, char const*, char const*, std::__cxx11::basic_string<char, std::char_traits<char>, std::allocator<char> >*, std::__cxx11::basic_string<char, std::char_traits<char>, std::allocator<char> >*)'
collect2: error: ld returned 1 exit status
Makefile:625: recipe for target '.build_release/tools/compute_image_mean.bin' failed
make: *** [.build_release/tools/compute_image_mean.bin] Error 1
```

```
$ sudo -i
# cd /dir to this caffe/python
# for req in $(cat requirements.txt); do pip install $req; done
# exit
$ cd ..
$ make pycaffe:
```

Re-install gflags and glog from official source accroding to [this page](https://github.com/google/glog/issues/140) and [this install guidance](https://github.com/gflags/gflags/blob/master/INSTALL.md).

```
cd repos
mkdir glog 
git clone https://github.com/google/glog
cd glog
./autogen.sh && ./configure && make && make install
```

### Reference
1. http://caffe.berkeleyvision.org/install_apt.html
2. http://caffe.berkeleyvision.org/installation.html#compilation
3. http://yufeigan.github.io/2014/12/14/Caffe%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B04-caffe%E5%AE%89%E8%A3%85%E9%9C%80%E8%A6%81%E6%B3%A8%E6%84%8F%E7%9A%84libraries/
4. https://github.com/BVLC/caffe/issues/2347
5. https://github.com/BVLC/caffe/issues/4333
6. https://github.com/google/glog



