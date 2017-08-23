# tensorflow r1.0.0 while python2.7
```
pip install --ignore-installed --upgrade https://storage.googleapis.com/tensorflow/linux/cpu/tensorflow-1.0.0-cp27-none-linux_x86_64.whl
```
# tensorflow r1.2.1 while python3.4
```
pip3 install --ignore-installed --upgrade https://storage.googleapis.com/tensorflow/linux/cpu/tensorflow-1.2.1-cp34-cp34m-linux_x86_64.whl
```
# tensorflow r1.3 while python2.7
```
git clone https://github.com/tensorflow/tensorflow.git
cd tensorflow
git checkout r1.3

./configure
```
install bazel

```
bazel build --config=opt //tensorflow/tools/pip_package:build_pip_package
bazel-bin/tensorflow/tools/pip_package/build_pip_package /tmp/tensorflow_pkg
[option: referent download this by terminal command wget LINK] 
[install protobuf](https://pypi.python.org/pypi/protobuf)
[install Markdown](https://pypi.python.org/pypi/Markdown)
pip install tensorflow-1.3.0rc2-cp27-cp27mu-linux_x86_64.whl
```

# Python 3.4.0
```
wget https://www.python.org/ftp/python/3.4.0/Python-3.4.0.tar.xz
xz -d Python-3.4.0.tar.xz
tar -xvf Python-3.4.0.tar
cd Python-3.4.0
./configure
make
sudo make install
```
* While installing Python 3.4.0 by  "sudo  make install" and getting an error like ignoring ensurepip failure: pip 1.5.6 requires SSL/TLS on Ubuntu 16.04 

```
$ sudo apt-get install libssl-dev openssl
```
* after py34 installed successfully
```
virtualenv -p /usr/local/bin/python3.4 vir-env
```
reference : http://www.jb51.net/article/101354.htm
