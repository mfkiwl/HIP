# Copyright (c) 2016 - 2021 Advanced Micro Devices, Inc. All Rights Reserved.
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

HIP_PATH?= $(wildcard /opt/rocm/hip)
ifeq (,$(HIP_PATH))
	HIP_PATH=../../..
endif
HIPCC=$(HIP_PATH)/bin/hipcc

EXE=hipCommander
OPT=-O3
#CXXFLAGS = -O3 -g
CXXFLAGS =  $(OPT) --std=c++11

HIP_PLATFORM=$(shell $(HIP_PATH)/bin/hipconfig --platform)

CODE_OBJECTS=nullkernel.hsaco

all: ${EXE} ${CODE_OBJECTS}

$(EXE): hipCommander.cpp 
	$(HIPCC) $(CXXFLAGS) $^ -o $@

nullkernel.hsaco : nullkernel.hip.cpp
	$(HIPCC) --genco nullkernel.hip.cpp -o nullkernel.hsaco


install: $(EXE)
	cp $(EXE) $(HIP_PATH)/bin


clean:
	rm -f *.o *.co $(EXE)
