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

PROGRAM = $(INSTALLDIR)/bin/onewifi_em_cli

INCLUDEDIRS = \
	-I$(ONEWIFI_EM_HOME)/inc \
	-I$(ONEWIFI_EM_HOME)/src/utils \
	-I$(ONEWIFI_EM_HOME)/src/util/ \
	-I$(ONEWIFI_HAL_INTF_HOME)/generic \
	-I$(ONEWIFI_HOME)/generic/source/utils \
	-I$(ONEWIFI_HOME)/generic/include \
    -I$(RBUS_HOME)/include \
	-I$(WIFI_CJSON) \

CXXFLAGS = $(INCLUDEDIRS) -g
LDFLAGS = $(LIBDIRS) $(LIBS)
LIBDIRS = \
	-L$(INSTALLDIR)/lib \

LIBS = -lm -lpthread -ldl -lcjson -lreadline

GENERIC_SOURCES = $(ONEWIFI_HOME)/generic/source/utils/collection.c

CLI_SOURCES = $(wildcard $(ONEWIFI_EM_SRC)/cli/*.cpp) \
	$(wildcard $(ONEWIFI_EM_SRC)/cmd/*.cpp) \
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

CLI_OBJECTS = $(CLI_SOURCES:.cpp=.o)
GENERIC_OBJECTS = $(GENERIC_SOURCES:.c=.o) 
ALLOBJECTS = $(CLI_OBJECTS) $(GENERIC_OBJECTS)

all: $(PROGRAM)

$(PROGRAM): $(ALLOBJECTS)
	$(CXX) -o $@ $(ALLOBJECTS) $(LDFLAGS)

$(GENERIC_OBJECTS): %.o: %.c
	$(CC) $(CXXFLAGS) -o $@ -c $<

$(CLI_OBJECTS): %.o: %.cpp
	$(CXX) $(CXXFLAGS) -o $@ -c $<

# Clean target: "make -f Makefile.Linux clean" to remove unwanted objects and executables.
#

clean:
	$(RM) $(ALLOBJECTS) $(PROGRAM)

#
# Run target: "make -f Makefile.Linux run" to execute the application
#             You will need to add $(VARIABLE_NAME) for any command line parameters 
#             that you defined earlier in this file.
# 

run:
	./$(PROGRAM) 

