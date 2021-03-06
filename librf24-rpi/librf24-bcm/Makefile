#############################################################################
#
# Makefile for librf24-bcm on Raspberry Pi
#
# License: GPL (General Public License)
# Author:  Charles-Henri Hallard 
# Date:    2013/03/13 
#
# Description:
# ------------
# use make all and mak install to install the library 
# You can change the install directory by editing the LIBDIR line
#
PREFIX=/usr/local

# Library parameters
# where to put the lib
LIBDIR=$(PREFIX)/lib
# lib name 
LIB_RFN=librf24network
# shared library name
LIBNAME_RFN=$(LIB_RFN).so.1.0

LIB_RF24=librf24-bcm
# shared library name
LIBNAME_RF24=$(LIB_RF24).so.1.0

# The recommended compiler flags for the Raspberry Pi
CCFLAGS=-Ofast -mfpu=vfp -mfloat-abi=hard -march=armv6zk -mtune=arm1176jzf-s

# make all
# reinstall the library after each recompilation
all: librf24-bcm install-rf24 librf24network install-rf24n

# Make the library
librf24network: RF24Network.o
	g++ -shared -Wl,-soname,$@.so.1 ${CCFLAGS} -o ${LIBNAME_RFN} $^ -lrf24-bcm

librf24-bcm: RF24.o bcm2835.o
	g++ -shared -Wl,-soname,$@.so.1 ${CCFLAGS} -o ${LIBNAME_RF24} $^

rx-example: rx-example.cpp
	g++ ${CCFLAGS} -Wall -lrf24-bcm -lrf24network $^ -o $@
	
# Library parts
RF24Network.o: RF24Network.cpp
	g++ -Wall -fPIC ${CCFLAGS} -c $^

RF24.o: RF24.cpp
	g++ -Wall -fPIC ${CCFLAGS} -c $^

bcm2835.o: bcm2835.c
	gcc -Wall -fPIC ${CCFLAGS} -c $^

# clear build files
clean:
	rm -rf *.o ${LIB_RF24}.* ${LIB_RFN}.*

# Install the library to LIBPATH
.PHONY: install-rf24n install-rf24
install-rf24n: 
	@echo "[Install]"
	@if ( test ! -d $(PREFIX)/lib ) ; then mkdir -p $(PREFIX)/lib ; fi
	@install -m 0755 ${LIBNAME_RFN} ${LIBDIR}
	@ln -sf ${LIBDIR}/${LIBNAME_RFN} ${LIBDIR}/${LIB_RFN}.so.1
	@ln -sf ${LIBDIR}/${LIBNAME_RFN} ${LIBDIR}/${LIB_RFN}.so
	@ldconfig

install-rf24: 
	@echo "[Install]"
	@if ( test ! -d $(PREFIX)/lib ) ; then mkdir -p $(PREFIX)/lib ; fi
	@install -m 0755 ${LIBNAME_RF24} ${LIBDIR}
	@ln -sf ${LIBDIR}/${LIBNAME_RF24} ${LIBDIR}/${LIB_RF24}.so.1
	@ln -sf ${LIBDIR}/${LIBNAME_RF24} ${LIBDIR}/${LIB_RF24}.so
	@ldconfig
install: install-r24 install-rf24n
	@echo "[Install]"