#MPI_FLAGS = -DUSE_MPI -I$(MPICH_DIR)/include

NVCC	= nvcc -ccbin clang-7
CPPFLAGS	= -x cu -dc -D_GLIBCXX_USE_CXX11_ABI=0
CPPFLAGS	+= -DDPCT # enables workaround DPCT needs
#CXXFLAGS	+= -arch=sm_60
CXXFLAGS	+= -gencode=arch=compute_60,code=\"sm_60,compute_60\"
CXXFLAGS	+= -std=c++11
DFLAGS	= $(MPI_FLAGS) -lineinfo
RFLAGS 	= $(MPI_FLAGS) -O3 -DNDEBUG

#SILO_INCLUDES := /usr/local/silo-4.8/include
#SILO_LIBS := /usr/local/silo-4.8/lib

#LINKFLAGS = -lmpich -L$(MPICH_DIR)/lib
#LINKFLAGS += -L$(SILO_LIBS) -lsilo
#LINKFLAGS += -std=c++11 -L/usr/lib/llvm-7/lib -lc++ -lm -lstdc++
LINKFLAGS += -lstdc++ -lm
LINKFLAGS += -L/usr/lib64/ -lcuda -lcudart

#INC_SILO:= -I$(SILO_INCLUDES)

all: release

debug: LINKFLAGS +=

release: 	FLAGS += $(RFLAGS)
debug: 		FLAGS += $(DFLAGS)

release: lulesh
debug: lulesh

lulesh: allocator.o lulesh.o lulesh-comms.o lulesh-comms-gpu.o
	$(NVCC) $(CXXFLAGS) allocator.o lulesh.o lulesh-comms.o lulesh-comms-gpu.o $(LINKFLAGS) -o lulesh

allocator.o: allocator.cu vector.h
	$(NVCC) $(CPPFLAGS) $(CXXFLAGS) allocator.cu -I ./ -c -o allocator.o

lulesh.o: lulesh.cu util.h vector.h allocator.h
	$(NVCC) $(CPPFLAGS) $(CXXFLAGS) lulesh.cu -I ./  $(INC_SILO) -c -o lulesh.o

lulesh-comms.o: lulesh-comms.cu
	$(NVCC) $(CPPFLAGS) $(CXXFLAGS) lulesh-comms.cu -I ./ -c -o lulesh-comms.o

lulesh-comms-gpu.o: lulesh-comms-gpu.cu
	$(NVCC) $(CPPFLAGS) $(CXXFLAGS) lulesh-comms-gpu.cu -I ./ -c -o lulesh-comms-gpu.o

clean:
	rm -rf allocator.o lulesh-comms.o lulesh-comms-gpu.o lulesh.o lulesh xyz.asc regNumList.txt
