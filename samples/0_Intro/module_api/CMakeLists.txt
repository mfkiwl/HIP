# Copyright (c) 2020 - 2021 Advanced Micro Devices, Inc. All Rights Reserved.
#
# Permission is hereby granted, free of charge, to any person obtaining a copy
# of this software and associated documentation files (the "Software"), to deal
# in the Software without restriction, including without limitation the rights
# to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
# copies of the Software, and to permit persons to whom the Software is
# furnished to do so, subject to the following conditions:
#
# The above copyright notice and this permission notice shall be included in
# all copies or substantial portions of the Software.
#
# THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
# IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
# FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.  IN NO EVENT SHALL THE
# AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
# LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
# OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
# THE SOFTWARE.

project(module_api)

cmake_minimum_required(VERSION 3.10)

# Search for rocm in common locations
list(APPEND CMAKE_PREFIX_PATH /opt/rocm/hip /opt/rocm)

# Find hip
find_package(hip)

# Set compiler and linker
set(CMAKE_CXX_COMPILER ${HIP_HIPCC_EXECUTABLE})

set(CMAKE_CXX_LINKER   ${HIP_HIPCC_EXECUTABLE})

# Create the excutable
add_executable(runKernel.hip.out runKernel.cpp)
add_executable(launchKernelHcc.hip.out launchKernelHcc.cpp)
add_executable(defaultDriver.hip.out defaultDriver.cpp)

# Generate code object
add_custom_target(
  codeobj
  ALL
  COMMAND  ${HIP_HIPCC_EXECUTABLE} --genco  ../vcpy_kernel.cpp -o vcpy_kernel.code
  COMMENT "codeobj generated"
)

add_dependencies(runKernel.hip.out codeobj)
add_dependencies(launchKernelHcc.hip.out codeobj)
add_dependencies(defaultDriver.hip.out codeobj)

# Link with HIP
target_link_libraries(runKernel.hip.out hip::host)
target_link_libraries(launchKernelHcc.hip.out hip::host)
target_link_libraries(defaultDriver.hip.out hip::host)
