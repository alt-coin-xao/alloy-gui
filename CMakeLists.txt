cmake_minimum_required(VERSION 3.16)

include("cmake/HunterGate.cmake")

set(HUNTER_NO_TOOLCHAIN_ID_RECALCULATION ON)
set(HUNTER_CONFIGURATION_TYPES Release)
set(HUNTER_STATUS_DEBUG OFF)
set(HUNTER_STATUS_PRINT ON)
set(HUNTER_PACKAGE_LOG_BUILD ON)
set(HUNTER_PACKAGE_LOG_INSTALL ON)
set(HUNTER_BUILD_SHARED_LIBS OFF)
set(HUNTER_KEEP_PACKAGE_SOURCE ON)
set(CMAKE_FIND_DEBUG_MODE ON)

#set_property(GLOBAL PROPERTY USE_FOLDERS ON)
#set(CMAKE_SKIP_INSTALL_RULES ON)
#set(CMAKE_SKIP_PACKAGE_ALL_DEPENDENCY ON)
#set(CMAKE_SUPPRESS_REGENERATION ON)

HunterGate(
    URL  "https://github.com/glencjones/hunter/archive/refs/tags/v0.23.314-alloy-1.tar.gz"
    SHA1 "4549223ba9d965c1cd332b2e99d566cfe010e06a"
)

execute_process(
  COMMAND git log -1 --pretty=format:%h
  WORKING_DIRECTORY ${CMAKE_SOURCE_DIR}
  OUTPUT_VARIABLE GIT_VERSION
)

add_definitions(-DGIT_REVISION="${GIT_VERSION}")

project(alloy-gui)

set(CN_PROJECT_NAME "AlloyWallet")
set(CN_CURRENCY_DISPLAY_NAME "Alloy")
set(CN_CURRENCY_TICKER "XAO")

set(CMAKE_BUILD_TYPE ${HUNTER_CONFIGURATION_TYPES})
set(BUILD_SHARED_LIBS ${HUNTER_BUILD_SHARED_LIBS})

###############################################################
###############################################################
##  Fetch Boot
##
#SET(Boost_USE_STATIC_LIBS ON)
#SET(Boost_USE_STATIC_RUNTIME ON)

hunter_add_package(Boost COMPONENTS program_options filesystem)
find_package(Boost CONFIG REQUIRED program_options filesystem)
###
###############################################################

###############################################################
###############################################################
## Fetch rocksdb
##
#SET(RocksDB_USE_STATIC_LIBS ON)

hunter_add_package(rocksdb)
find_package(RocksDB CONFIG REQUIRED)
###
###############################################################

###############################################################
###############################################################
## Fetch miniupnpc
##
#SET(miniupnpc_USE_STATIC_LIBS ON)

hunter_add_package(miniupnpc)
find_package(miniupnpc CONFIG REQUIRED)
###
###############################################################

###############################################################
###############################################################
## Fetch miniupnpc
###
#find_package(Qt CONFIG REQUIRED COMPONENTS Gui Widgets Network DBus)
#
SET(CMAKE_AUTOMOC ON)
find_package(Qt5 COMPONENTS Gui Widgets Network DBus REQUIRED)
###
###############################################################

###############################################################
###############################################################
## Fetch Alloy build to use static libs
## TODO Remove build alloyd don't really need it here
##
include(ExternalProject)

ExternalProject_Add(alloylibs
  URL https://github.com/alt-coin-xao/alloy/archive/refs/tags/v.3.2-beta.tar.gz
  URL_HASH MD5=0256c480f509606ad8636bc70af177dc
  INSTALL_COMMAND ""
  STEP_TARGETS build
  )
###
##################################################################

add_subdirectory(src)

##################################################################
##################################################################
## Tests are not working will fix in next release, seems to be
## previous code issues again.
## TODO Place correct option handling around tests
#add_subdirectory(tests)
###
##################################################################
