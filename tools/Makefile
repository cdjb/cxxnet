# set LD_LIBRARY_PATH
export CC  = gcc
export CXX = g++
export NVCC =nvcc

#export CFLAGS = -Wall -g -O3 -msse3 -Wno-unknown-pragmas -funroll-loops -I./mshadow/

#ifeq ($(blas),1)
# LDFLAGS= -lm -lcudart -lcublas -lcurand -lz `pkg-config --libs opencv` -lblas
# CFLAGS+= -DMSHADOW_USE_MKL=0 -DMSHADOW_USE_CBLAS=1 -pthread
#else
# LDFLAGS= -lm -lcudart -lcublas -lmkl_core -lmkl_intel_lp64 -lmkl_intel_thread -liomp5 -lpthread -lcurand -lz `pkg-config --libs opencv`
#endif

#export NVCCFLAGS = --use_fast_math -g -O3 -ccbin $(CXX)

export CFLAGS = -Wall -O3 -msse3 -Wno-unknown-pragmas -funroll-loops -I../mshadow/ -I..

CFLAGS+=-I/usr/local/cuda/include
export LDFLAGS=-L/usr/local/cuda/lib64 -lm -lcudart -lcublas -lcurand -lz `pkg-config --libs opencv` -lblas
export NVCCFLAGS = -g -O3 -ccbin $(CXX)
CFLAGS+= -DMSHADOW_USE_MKL=0 -DMSHADOW_USE_CBLAS=1 -pthread

# specify tensor path
BIN = im2bin
OBJ =
CUOBJ =
CUBIN =
.PHONY: clean all

all: $(BIN) $(OBJ) $(CUBIN) $(CUOBJ)

im2bin: im2bin.cpp
$(BIN) :
	$(CXX) $(CFLAGS) -o $@ $(filter %.cpp %.o %.c, $^)  $(LDFLAGS)

$(OBJ) :
	$(CXX) -c $(CFLAGS) -o $@ $(firstword $(filter %.cpp %.c, $^) )

$(CUOBJ) :
	$(NVCC) -c -o $@ $(NVCCFLAGS) -Xcompiler "$(CFLAGS)" $(filter %.cu, $^)
$(CUBIN) :
	$(NVCC) -o $@ $(NVCCFLAGS) -Xcompiler "$(CFLAGS)" -Xlinker "$(LDFLAGS)" $(filter %.cu %.cpp %.o, $^)

clean:
	$(RM) $(OBJ) $(BIN) $(CUBIN) $(CUOBJ) *~

