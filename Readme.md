# LFX-Mentorship-Proposal-3170

## OutLine
[toc]

## About
This is a repository of LFX pretest

## whisper.cpp




## Build WasmEdge with WASI-NN llama.cpp Backend
Since we need to build from the source, 
1. Clone the repository from the github, and use the specific version
```bash
$ git clone https://github.com/WasmEdge/WasmEdge.git
$ git checkout hydai/0.13.5_ggml_lts
```

2.  Install the dependencies
```bash
apt update
apt install -y software-properties-common lsb-release cmake unzip pkg-config ninja-build g++

# Install llvm
wget https://apt.llvm.org/llvm.sh
chmod +x llvm.sh
sudo ./llvm.sh 13
```

3. install CUDA
```bash
wget https://developer.download.nvidia.com/compute/cuda/repos/wsl-ubuntu/x86_64/cuda-keyring_1.1-1_all.deb
sudo dpkg -i cuda-keyring_1.1-1_all.deb
sudo apt-get update
sudo apt-get -y install cuda
```

4. Build WasmEdge 
```bash
# Due to cuda-related files, it will produce some warning.
# Disable the warning as an error to avoid failures.
export CXXFLAGS="-Wno-error"
# Please make sure you set up the correct CUDAARCHS.
# We use `60;61;70` for maximum compatibility.
export CUDAARCHS="60;61;70"

# BLAS cannot work with CUBLAS
cmake -GNinja -Bbuild -DCMAKE_BUILD_TYPE=Release \
  -DCMAKE_CUDA_ARCHITECTURES="60;61;70" \
  -DCMAKE_CUDA_COMPILER=/usr/local/cuda/bin/nvcc \
  -DWASMEDGE_PLUGIN_WASI_NN_BACKEND="GGML" \
  -DWASMEDGE_PLUGIN_WASI_NN_GGML_LLAMA_BLAS=OFF \
  -DWASMEDGE_PLUGIN_WASI_NN_GGML_LLAMA_CUBLAS=ON \
  .
```
