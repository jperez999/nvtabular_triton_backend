<!--
# Copyright (c) 2020, NVIDIA CORPORATION. All rights reserved.
#
# Redistribution and use in source and binary forms, with or without
# modification, are permitted provided that the following conditions
# are met:
#  * Redistributions of source code must retain the above copyright
#    notice, this list of conditions and the following disclaimer.
#  * Redistributions in binary form must reproduce the above copyright
#    notice, this list of conditions and the following disclaimer in the
#    documentation and/or other materials provided with the distribution.
#  * Neither the name of NVIDIA CORPORATION nor the names of its
#    contributors may be used to endorse or promote products derived
#    from this software without specific prior written permission.
#
# THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS ``AS IS'' AND ANY
# EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE
# IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR
# PURPOSE ARE DISCLAIMED.  IN NO EVENT SHALL THE COPYRIGHT OWNER OR
# CONTRIBUTORS BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL,
# EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO,
# PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR
# PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY
# OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT
# (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE
# OF THIS SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.
-->

[![License](https://img.shields.io/badge/License-Apache%202.0-lightgrey.svg)](https://opensource.org/licenses/Apache-2.0)

# NVTabular C++ Triton Backend
This repo includes the source code for the NVTabular C++ Triton Backend.

`nvcr.io/nvstaging/merlin/merlin-inference` container includes all the required
packages and libraries to build the C++ backend from source. Please follow the
steps below;

```
$ conda install -c conda-forge rapidjson pybind11 cmake
$ git clone https://github.com/NVIDIA-Merlin/nvtabular_triton_backend.git
$ cd nvtabular_triton_backend/
$ mkdir build
$ cd build
$ cmake ..
$ make install
$ mkdir /opt/tritonserver/nvtabular
$ cp libtriton_nvtabular.so /opt/tritonserver/nvtabular/
```

Before start serving a model with nvtabular backend, run the following command;

```
$ export LD_LIBRARY_PATH=/conda/envs/merlin/lib/:$LD_LIBRARY_PATH
```

If the path of the libraries (i.e. python libs) has already been added to the `LD_LIBRARY_PATH`,
you don't have to run the command above.

The model that will use NVTabular C++ backend should have the following in the
config.pbtxt file

```
backend: "nvtabular"
instance_group [{ kind: KIND_CPU }]
```

Note that the following required Triton repositories will be pulled and used in
the build. By default the "main" branch/tag will be used for each repo
but the listed CMake argument can be used to override.

* triton-inference-server/backend: -DTRITON_BACKEND_REPO_TAG=[tag]
* triton-inference-server/core: -DTRITON_CORE_REPO_TAG=[tag]
* triton-inference-server/common: -DTRITON_COMMON_REPO_TAG=[tag]
