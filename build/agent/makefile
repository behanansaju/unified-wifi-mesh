##########################################################################
 # Copyright 2023 Comcast Cable Communications Management, LLC
 #
 # Licensed under the Apache License, Version 2.0 (the "License");
 # you may not use this file except in compliance with the License.
 # You may obtain a copy of the License at
 #
 # http://www.apache.org/licenses/LICENSE-2.0
 #
 # Unless required by applicable law or agreed to in writing, software
 # distributed under the License is distributed on an "AS IS" BASIS,
 # WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
 # See the License for the specific language governing permissions and
 # limitations under the License.
 #
 # SPDX-License-Identifier: Apache-2.0
##########################################################################

include ../makefile.inc

#
# program
#

BUS_LIBRARY = $(INSTALLDIR)/lib/libbus.a
PROGRAM = $(INSTALLDIR)/bin/onewifi_em_agent

INCLUDEDIRS = \
	-I$(ONEWIFI_EM_HOME)/inc \
	-I$(ONEWIFI_EM_HOME)/src/utils \
	-I$(ONEWIFI_HAL_INTF_HOME)/generic \
	-I$(ONEWIFI_HOME)/generic/source/utils \
	-I$(ONEWIFI_HOME)/generic/include \
    	-I$(ONEWIFI_BUS_LIB_HOME)/inc \
    	-I$(ONEWIFI_HOME)/generic/lib/log \
    	-I$(ONEWIFI_HOME)/generic/lib/ds \
    	-I$(ONEWIFI_HOME)/generic/source/platform/linux \
    	-I$(ONEWIFI_HOME)/generic/source/platform/common \
    	-I$(ONEWIFI_HOME)/generic/source/platform/linux/he_bus/inc \
        -I$(ONEWIFI_EM_HOME)/src/util_crypto \

$(info The value of VARIABLE is $(ONEWIFI_HAL_INTF_HOME))

CXXFLAGS = $(INCLUDEDIRS) -g
LDFLAGS = $(LIBDIRS) $(LIBS)
LIBDIRS = \
	-L$(INSTALLDIR)/lib \
    -L$(ONEWIFI_HOME)/install/lib/

LIBS = -lm -lpthread -ldl -lcjson -luuid -lssl -lcrypto -lbus -lwebconfig -lhebus

GENERIC_SOURCES = $(ONEWIFI_HOME)/generic/source/utils/collection.c \
	$(ONEWIFI_EM_SRC)/util_crypto/aes_siv.c \
    	$(ONEWIFI_HOME)/generic/lib/common/util.c \
    	$(ONEWIFI_HOME)/generic/source/platform/linux/bus.c \

AGENT_SOURCES = $(wildcard $(ONEWIFI_EM_SRC)/em/*.cpp) \
	$(wildcard $(ONEWIFI_EM_SRC)/em/config/*.cpp) \
	$(wildcard $(ONEWIFI_EM_SRC)/em/prov/*.cpp) \
	$(wildcard $(ONEWIFI_EM_SRC)/em/disc/*.cpp) \
	$(wildcard $(ONEWIFI_EM_SRC)/em/channel/*.cpp) \
	$(wildcard $(ONEWIFI_EM_SRC)/em/capability/*.cpp) \
	$(wildcard $(ONEWIFI_EM_SRC)/em/metrics/*.cpp) \
	$(wildcard $(ONEWIFI_EM_SRC)/em/steering/*.cpp) \
	$(wildcard $(ONEWIFI_EM_SRC)/em/crypto/*.cpp) \
    	$(ONEWIFI_EM_SRC)/orch/em_orch.cpp \
    	$(ONEWIFI_EM_SRC)/orch/em_orch_agent.cpp \
	$(wildcard $(ONEWIFI_EM_SRC)/cmd/*.cpp) \
	$(wildcard $(ONEWIFI_EM_SRC)/agent/*.cpp) \
	$(ONEWIFI_EM_SRC)/dm/dm_device.cpp \
	$(ONEWIFI_EM_SRC)/dm/dm_ieee_1905_security.cpp \
	$(ONEWIFI_EM_SRC)/dm/dm_easy_mesh.cpp \
	$(ONEWIFI_EM_SRC)/dm/dm_radio.cpp \
	$(ONEWIFI_EM_SRC)/dm/dm_bss.cpp \
	$(ONEWIFI_EM_SRC)/dm/dm_dpp.cpp \
	$(ONEWIFI_EM_SRC)/dm/dm_network_ssid.cpp \
	$(ONEWIFI_EM_SRC)/dm/dm_network.cpp \
	$(ONEWIFI_EM_SRC)/dm/dm_op_class.cpp \
	$(ONEWIFI_EM_SRC)/dm/dm_sta.cpp \
	$(ONEWIFI_EM_SRC)/dm/dm_radio_cap.cpp \

BUS_LIBRARY_SOURCES = $(wildcard $(ONEWIFI_BUS_LIB_HOME)/src/*.c) \

BUS_LIBRARY_OBJECTS = $(BUS_LIBRARY_SOURCES:.c=.o)
AGENT_OBJECTS = $(AGENT_SOURCES:.cpp=.o)
GENERIC_OBJECTS = $(GENERIC_SOURCES:.c=.o) 
ALLOBJECTS = $(AGENT_OBJECTS) $(GENERIC_OBJECTS)

all: $(BUS_LIBRARY) $(PROGRAM)

$(BUS_LIBRARY): $(BUS_LIBRARY_OBJECTS)
	$(AR) $@ $^

$(PROGRAM): $(ALLOBJECTS)
	$(CXX) -o $@ $(ALLOBJECTS) $(LDFLAGS)

$(BUS_LIBRARY_OBJECTS): %.o: %.c
	$(CC) $(CXXFLAGS) -o $@ -c $<

$(GENERIC_OBJECTS): %.o: %.c
	$(CC) $(CXXFLAGS) -o $@ -c $<

$(AGENT_OBJECTS): %.o: %.cpp
	$(CXX) $(CXXFLAGS) -o $@ -c $<

# Clean target: "make -f Makefile.Linux clean" to remove unwanted objects and executables.
#

clean:
	$(RM) $(ALLOBJECTS) $(PROGRAM) $(BUS_LIBRARY_OBJECTS) $(BUS_LIBRARY)

#
# Run target: "make -f Makefile.Linux run" to execute the application
#             You will need to add $(VARIABLE_NAME) for any command line parameters 
#             that you defined earlier in this file.
# 

run:
	./$(PROGRAM) 

