# ##############################################################################
# apps/examples/fdpicxip/CMakeLists.txt
#
# SPDX-License-Identifier: Apache-2.0
#
# Licensed to the Apache Software Foundation (ASF) under one or more contributor
# license agreements.  See the NOTICE file distributed with this work for
# additional information regarding copyright ownership.  The ASF licenses this
# file to you under the Apache License, Version 2.0 (the "License"); you may not
# use this file except in compliance with the License.  You may obtain a copy of
# the License at
#
# http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS, WITHOUT
# WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.  See the
# License for the specific language governing permissions and limitations under
# the License.
#
# ##############################################################################

nuttx_add_subdirectory()

if(CONFIG_EXAMPLES_FDPICXIP)

  # The modules are built in modules/, and embedded here after they are linked

  set(BLOBSRC ${CMAKE_CURRENT_BINARY_DIR}/fdpicxip_blobs.c)
  set(BLOBFILES)
  set(BLOBTARGETS)

  foreach(blob qsorter libcounter.so user libshape.so cxxuser lazymod)
    list(APPEND BLOBFILES ${NUTTX_BINARY_DIR}/bin/${blob})

    if(CMAKE_C_ELF_COMPILER)
      list(APPEND BLOBTARGETS ${blob})
    else()
      list(APPEND BLOBTARGETS ELF_${blob})
    endif()
  endforeach()

  add_custom_command(
    OUTPUT ${BLOBSRC}
    COMMAND ${CMAKE_CURRENT_SOURCE_DIR}/modules/mkblobs.sh g_fdpicxip
            ${BLOBFILES} > ${BLOBSRC}
    DEPENDS ${BLOBTARGETS})

  add_custom_target(fdpicxip_blobs DEPENDS ${BLOBSRC})

  nuttx_add_application(
    NAME
    fdpicxip
    SRCS
    fdpicxip_main.c
    ${BLOBSRC}
    DEPENDS
    fdpicxip_blobs)
endif()
