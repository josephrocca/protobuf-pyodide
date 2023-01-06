# Protobuf Pyodide
Protobuf build instructions for Pyodide and simple demo of it running in the browser (this will likely become obsolete soon as it'll be integrated into the Pyodide package listing if all goes well)

* whl: https://github.com/josephrocca/protobuf-pyodide/raw/main/demo/protobuf-4.21.12-py2.py3-none-any.whl
* demo: https://josephrocca.github.io/protobuf-pyodide/demo

# Build:

```bash
git clone --single-branch v21.12 https://github.com/protocolbuffers/protobuf
cd protobuf

# pyodide setup:
pip install pyodide-build
git clone https://github.com/emscripten-core/emsdk
cd emsdk
pyodide config get emscripten_version  # this line is needed due to: https://github.com/pyodide/pyodide/issues/3430
./emsdk install $(pyodide config get emscripten_version)
./emsdk activate $(pyodide config get emscripten_version)
source emsdk_env.sh
cd ../

# Install `protoc` binary - https://askubuntu.com/questions/1072683/how-can-i-install-protoc-on-ubuntu-16-04/1157801#1157801
# CAUTION: Must be same version as the protobuf repo being built!
wget https://github.com/protocolbuffers/protobuf/releases/download/v21.12/protoc-21.12-linux-x86_64.zip
sudo unzip -o protoc-21.12-linux-x86_64.zip -d /usr/local bin/protoc
sudo unzip -o protoc-21.12-linux-x86_64.zip -d /usr/local 'include/*'

# install bazel:
npm install -g @bazel/bazelisk

# build
cd python
pyodide build

# create test file:
wget https://gist.githubusercontent.com/josephrocca/f9184834444be2515b264aca2c9e7fde/raw/cc25a18b1e4d9a71bd99c8343bd31bfd43116e85/addressbook.proto
protoc -I=. --python_out=. addressbook.proto
```
