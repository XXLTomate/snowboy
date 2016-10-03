# Example Makefile that converts snowboy c++ library (snowboy-detect.a) to
# java library (libsnowboydetect.so, snowboy.java, snowboyJNI.java), using swig.

# Please use swig-3.0.10 or up.
SWIG := swig

SNOWBOYDETECTSWIGITF = snowboy-detect-swig.i
SNOWBOYDETECTSWIGOBJ = snowboy-detect-swig.o
SNOWBOYDETECTSWIGCC = snowboy-detect-swig.cc
SNOWBOYDETECTSWIGLIBFILE = libsnowboydetect.so


#TODO: JAVAPackage
JAVAHOME := /usr/lib/jvm/java-8-openjdk-amd64
ATLASPATH := /usr/lib/atlas-base
TOPDIR := ../..
CXXFLAGS := -I$(TOPDIR) -I$(JAVAHOME)/include -I$(JAVAHOME)/include/linux -O3 -fPIC -D_GLIBCXX_USE_CXX11_ABI=0
LDFLAGS :=


CXX := g++
SWIGFLAGS := -shared
CXXFLAGS += -std=c++0x
# Make sure you have Atlas installed. You can statically link Atlas if you
# would like to be able to move the library to a machine without Atlas.
LDLIBS := -lm -ldl -lf77blas -lcblas -llapack_atlas -latlas
SNOWBOYDETECTLIBFILE = $(TOPDIR)/lib/ubuntu64/libsnowboy-detect.a
ifneq (,$(findstring arm,$(shell uname -m)))
SNOWBOYDETECTLIBFILE = $(TOPDIR)/lib/rpi/libsnowboy-detect.a
endif


all: $(SNOWBOYSWIGLIBFILE) $(SNOWBOYDETECTSWIGLIBFILE)

%.a:
	$(MAKE) -C ${@D} ${@F}

$(SNOWBOYDETECTSWIGCC): $(SNOWBOYDETECTSWIGITF)
	$(SWIG) -I$(TOPDIR) -c++ -java -o $(SNOWBOYDETECTSWIGCC) $(SNOWBOYDETECTSWIGITF)

$(SNOWBOYDETECTSWIGOBJ): $(SNOWBOYDETECTSWIGCC)
	$(CXX) $(CXXFLAGS) -c $(SNOWBOYDETECTSWIGCC) 

$(SNOWBOYDETECTSWIGLIBFILE): $(SNOWBOYDETECTSWIGOBJ) $(SNOWBOYDETECTLIBFILE)
	$(CXX) $(CXXFLAGS) $(LDFLAGS) $(SWIGFLAGS) $(SNOWBOYDETECTSWIGOBJ) \
	$(SNOWBOYDETECTLIBFILE) $(LDLIBS) -L$(ATLASPATH) -o $(SNOWBOYDETECTSWIGLIBFILE)

clean:
	-rm -f *.o *.a *.so *.java $(SNOWBOYDETECTSWIGCC)
