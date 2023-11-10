SHELL = /bin/sh
.SUFFIXES: .cc .o

LULESH_EXEC = lulesh2.0
CXX = clang++

SOURCES2.0 = \
	lulesh.cc \
	lulesh-comm.cc \
	lulesh-viz.cc \
	lulesh-util.cc \
	lulesh-init.cc
OBJECTS2.0 = $(SOURCES2.0:.cc=.o)

hpx_install_lib = $(HPX_INSTALL_LIB)
hpx_install_include = $(HPX_INSTALL_INCLUDE)
llvm_install_include = $(LLVM_INSTALL_INCLUDE)

teams = 
ifdef TEAMS
teams = -DTEAMS=$(TEAMS)
endif

threads = 
ifdef THREADS
threads = -DTHREADS=$(THREADS)
endif

gpu = -DUSE_GPU=0
mpi = -DUSE_MPI=0

LDFLAGS = -std=c++17  -L$(hpx_install_lib)  -Wl,-rpath=$(hpx_install_lib)

CXXFLAGS = -std=c++17 -fopenmp -g -O3 -DHPX -I$(hpx_install_include) -I$(llvm_install_include) $(teams) $(threads) $(gpu) $(mpi)

.cc.o: lulesh.h
	@echo "Building $<"
	$(CXX) -c $(CXXFLAGS) -o $@  $<

all: $(LULESH_EXEC)

lulesh2.0: $(OBJECTS2.0)
	@echo "Linking"
	$(CXX) $(OBJECTS2.0) $(LDFLAGS) -l:libhpx_init.a -lhpx -lhpx_core -lomp -lc++ -lm  -o $@

clean:
	/bin/rm -f *.o *~ *.tgt* $(OBJECTS) $(LULESH_EXEC)
	/bin/rm -rf *.dSYM

tar: clean
	cd .. ; tar cvf lulesh-2.0.tar LULESH-2.0 ; mv lulesh-2.0.tar LULESH-2.0
