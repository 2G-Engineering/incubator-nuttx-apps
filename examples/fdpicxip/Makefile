############################################################################
# apps/examples/fdpicxip/Makefile
#
# SPDX-License-Identifier: Apache-2.0
#
# Licensed to the Apache Software Foundation (ASF) under one or more
# contributor license agreements.  See the NOTICE file distributed with
# this work for additional information regarding copyright ownership.  The
# ASF licenses this file to you under the Apache License, Version 2.0 (the
# "License"); you may not use this file except in compliance with the
# License.  You may obtain a copy of the License at
#
#   http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS, WITHOUT
# WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.  See the
# License for the specific language governing permissions and limitations
# under the License.
#
############################################################################

include $(APPDIR)/Make.defs

# FDPIC modules executed in place from xipfs.  The modules are built in
# modules/, and embedded here after they are linked.

MAINSRC   = fdpicxip_main.c

PROGNAME  = fdpicxip
PRIORITY  = SCHED_PRIORITY_DEFAULT
STACKSIZE = $(CONFIG_DEFAULT_TASK_STACKSIZE)
MODULE    = $(CONFIG_EXAMPLES_FDPICXIP)

BLOBS   = qsorter libcounter.so user libshape.so cxxuser lazymod
BLOBSRC = fdpicxip_blobs.c
BLOBOBJ = $(BLOBSRC:.c=$(OBJEXT))

$(BLOBSRC): $(addprefix $(BINDIR)$(DELIM),$(BLOBS))
	$(Q) modules$(DELIM)mkblobs.sh g_fdpicxip $^ > $@.tmp
	$(Q) $(call TESTANDREPLACEFILE, $@.tmp, $@)

$(BLOBOBJ): %$(OBJEXT): %.c
	$(call COMPILE, $<, $@)

postinstall:: $(BLOBOBJ)
	$(call ARLOCK, $(call CONVERT_PATH,$(BIN)), $^)

clean::
	$(Q) $(MAKE) -C modules clean APPDIR="$(APPDIR)" TOPDIR="$(TOPDIR)"

distclean::
	$(Q) $(MAKE) -C modules distclean APPDIR="$(APPDIR)" TOPDIR="$(TOPDIR)"
	$(call DELFILE, $(BLOBSRC) $(BLOBOBJ))

include $(APPDIR)/Application.mk
