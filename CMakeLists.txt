# Copyright 2010, 2011, Florent Lamiraux, Thomas Moulard, JRL, CNRS/AIST
#
# This file is part of dynamic-graph-python.
# dynamic-graph-python is free software: you can redistribute it and/or
# modify it under the terms of the GNU Lesser General Public License
# as published by the Free Software Foundation, either version 3 of
# the License, or (at your option) any later version.
#
# dynamic-graph-python is distributed in the hope that it will be useful, but
# WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the GNU
# General Lesser Public License for more details.  You should have
# received a copy of the GNU Lesser General Public License along with
# dynamic-graph-python. If not, see <http://www.gnu.org/licenses/>.

CMAKE_MINIMUM_REQUIRED(VERSION 2.8.3)

project(dynamic-graph-python)
find_package(catkin REQUIRED COMPONENTS sot_cmake dynamic-graph)

link_directories(${DYNAMIC_GRAPH_PLUGINDIR})

INCLUDE(${SOT_CMAKE_DIR}/base.cmake)
INCLUDE(${SOT_CMAKE_DIR}/boost.cmake)
INCLUDE(${SOT_CMAKE_DIR}/pthread.cmake)
INCLUDE(${SOT_CMAKE_DIR}/cpack.cmake)
include(${SOT_CMAKE_DIR}/header.cmake)

SET(PROJECT_DESCRIPTION "Dynamic graph library Python bindings")
SET(PROJECT_URL "http://github.com/jrl-umi3218/dynamic-graph-python")

SET(CUSTOM_HEADER_DIR "dynamic-graph/python")
# Headers list.
SET(${PROJECT_NAME}_HEADERS
  include/dynamic-graph/python/interpreter.hh
  include/dynamic-graph/python/api.hh
  include/dynamic-graph/python/exception-python.hh

)

SET(PKG_CONFIG_ADDITIONAL_VARIABLES plugindir ${PKG_CONFIG_ADDITIONAL_VARIABLES})

SETUP_PROJECT()

catkin_package(
  INCLUDE_DIRS include
  LIBRARIES ${PROJECT_NAME}
  CATKIN_DEPENDS dynamic-graph
)

catkin_add_env_hooks(
  20.dynamic-graph-python
  SHELLS bash
  DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/env-hooks
)

include_directories(include ${catkin_INCLUDE_DIRS})

# Trigger dependency to dynamic-graph.
#ADD_REQUIRED_DEPENDENCY("dynamic-graph >= 2.5.5-6")

# Add dependency toward dynamic graph library in pkg-config file.
PKG_CONFIG_APPEND_LIBS("dynamic-graph-python")

# Search for Boost.
SEARCH_FOR_BOOST()

# Make sure Boost.Filesystem v2 is used.
ADD_DEFINITIONS(-DBOOST_FILESYSTEM_VERSION=2)

ADD_SUBDIRECTORY(src)
ADD_SUBDIRECTORY(include)
#ADD_SUBDIRECTORY(doc)
#ADD_SUBDIRECTORY(unitTesting)

SETUP_PROJECT_FINALIZE()

