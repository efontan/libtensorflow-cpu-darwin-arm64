# libtensorflow-cpu-darwin-arm64
Tensorflow C binaries compiled for Mac M1 computers (arm64 chip).

The libraries were compiled with all flags disabled:

```bash
MACOSX_DEPLOYMENT_TARGET=12.0 ./configure

Do you wish to build TensorFlow with ROCm support? [y/N]: N
No ROCm support will be enabled for TensorFlow.

Do you wish to build TensorFlow with CUDA support? [y/N]: N
No CUDA support will be enabled for TensorFlow.

Do you wish to download a fresh release of clang? (Experimental) [y/N]: N
Clang will not be downloaded.

Please specify optimization flags to use during compilation when bazel option "--config=opt" is specified [Default is -Wno-sign-compare]:

Would you like to interactively configure ./WORKSPACE for Android builds? [y/N]: N
Not configuring the WORKSPACE for Android builds.

Do you wish to build TensorFlow with iOS support? [y/N]: N
No iOS support will be enabled for TensorFlow.
```

Build command:

```bash
bazel build --config=macos_arm64 --config=noaws --config=nogcp --config=nohdfs --config=nonccl //tensorflow/tools/lib_package:libtensorflow
```

# How to build Tensorflow binaries from source?
- Install [Bazelisk](https://github.com/bazelbuild/bazelisk#installation)
```
brew install bazelisk
```
- Create a virtual environment with Python 3.9, using [virtualenvwrapper](https://github.com/bernardobarreto/virtualenvwrapper):
  - Install Python 3.9:
  ```
  wget https://www.python.org/ftp/python/3.9.13/python-3.9.13-macosx10.9.pkg
  sudo installer -pkg python-3.9.13-macosx10.9.pkg -target /
  rm python-3.9.13-macosx10.9.pkg
  ```
  - Install virtualenvwrapper:
  ```
  pip install virtualenvwrapper
  mkdir -p $HOME/.virtualenvs
  echo 'export WORKON_HOME=~/virtualenvs' >> ~/.zshrc # or ./bashrc
  echo 'export VIRTUALENVWRAPPER_VIRTUALENV=$(which virtualenv)' >> ~/.zshrc
  echo 'export VIRTUALENVWRAPPER_PYTHON=$(which python3.9)' >> ~/.zshrc
  echo 'source $(which virtualenvwrapper.sh)' >> ~/.zshrc
  source ~/.zshrc
  ```
  - Create a virtual environment using Python 3.9:
  ```
  mkvirtualenv tf-build --python=$(which python3.9)
  workon tf-build
  python -m pip install --upgrade pip
  ```
- Install Tensorflow dependencies:
```
pip install -U --user pip numpy wheel packaging requests opt_einsum
pip install -U --user keras_preprocessing --no-deps
```
- Clone repo:
```
git clone git@github.com:tensorflow/tensorflow.git
cd tensorflow
git checkout v${VERSION_NUMBER} #example v2.9.0
```
- Configure and build:
```
./configure
bazel build --config=macos_arm64 --config=noaws --config=nogcp --config=nohdfs --config=nonccl //tensorflow/tools/lib_package:libtensorflow
```
- Rename and copy the tar file:
```
cp bazel-bin/tensorflow/tools/lib_package/libtensorflow.tar.gz ./libtensorflow-cpu-darwin-arm64-2.9.0.tar.gz
```

