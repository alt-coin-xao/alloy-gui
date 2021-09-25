ExternalProject_Get_Property(alloylibs SOURCE_DIR)
MESSAGE(STATUS "#############################################################")
MESSAGE(STATUS "#############################################################")
MESSAGE(STATUS "#############################################################")
MESSAGE(STATUS "#############################################################")
MESSAGE(STATUS "#############################################################")
MESSAGE(STATUS "#############################################################")
MESSAGE(STATUS "#############################################################")
MESSAGE(STATUS "#############################################################")
MESSAGE(STATUS "#############################################################")
MESSAGE(STATUS "#############################################################")
MESSAGE(STATUS "Src ${SOURCE_DIR}")
ExternalProject_Get_Property(alloylibs BINARY_DIR)
MESSAGE(STATUS "Bin ${BINARY_DIR}")


# libBlockchainExplorer.a
# libCommon.a
# libCrypto.a
# libCryptoNoteCore.a
# libHttp.a
# libInProcessNode.a
# libJsonRpcServer.a
# libLogging.a
# libNodeRpcProxy.a
# libP2P.a
# libPaymentGate.a
# libRpc.a
#  libSerialization.a
# libSystem.a
# libTransfers.a
# libWallet.a

set(CMAKE_AUTOMOC ON)
set_property(SOURCE qrc_resources.cpp PROPERTY SKIP_AUTOMOC ON)

#INCLUDE_DIRECTORIES(QT5::Gui QT5::Widgets QT5::Network QT5::DBus))
################################################################
################################################################
### Project Fikes
###
file(GLOB_RECURSE SOURCES ${CMAKE_SOURCE_DIR}src/*.cpp)
file(GLOB_RECURSE HEADERS ${CMAKE_SOURCE_DIR}src/*.h)
file(GLOB_RECURSE FORMS ${CMAKE_SOURCE_DIR}src/Gui/*.ui)
###
################################################################

################################################################
################################################################
### Environment Specific
###
if(MSVC)
  include_directories(Platform/Windows)
  set(SOURCES ${SOURCES} ${CMAKE_SOURCE_DIR}/Platform/Windows/ApplicationEventHandler.cpp)
elseif(APPLE)
  include_directories(Platform/OSX)
  include_directories(Platform/Posix)
  set(SOURCES ${SOURCES} ${CMAKE_SOURCE_DIR}/Platform/OSX/ApplicationEventHandler.mm)
  set(SOURCES ${SOURCES} ${CMAKE_SOURCE_DIR}/Platform/Posix/System/MemoryMappedFile.cpp)
else()
  include_directories(Platform/Linux)
  include_directories(Platform/Posix)
  set(SOURCES ${SOURCES} ${CMAKE_SOURCE_DIR}/Platform/Linux/ApplicationEventHandler.cpp)
  set(SOURCES ${SOURCES} ${CMAKE_SOURCE_DIR}/Platform/Posix/System/MemoryMappedFile.cpp)
endif()

if (APPLE)
# qt5_use_modules(${PROJECT_NAME} PrintSupport)
elseif (UNIX)
#  target_link_libraries(${PROJECT_NAME} -lpthread)
elseif (WIN32)
#  target_link_libraries(${PROJECT_NAME} Imm32 Iphlpapi Winmm UxTheme d3d9 dxguid Rpcrt4 Wtsapi32 Dwmapi "${_qt5Widgets_install_prefix}/lib/qtharfbuzz.lib" "${_qt5Widgets_install_prefix}/lib/qtlibpng.lib" "${_qt5Widgets_install_prefix}/lib/qtpcre.lib" "${_qt5Widgets_install_prefix}/lib/Qt5EventDispatcherSupport.lib" "${_qt5Widgets_install_prefix}/lib/Qt5OpenGL.lib" "${_qt5Widgets_install_prefix}/lib/Qt5AccessibilitySupport.lib" "${_qt5Widgets_install_prefix}/lib/Qt5ThemeSupport.lib" "${_qt5Widgets_install_prefix}/lib/libGLESv2.lib" "${_qt5Widgets_install_prefix}/lib/libEGL.lib" "${_qt5Widgets_install_prefix}/lib/qtfreetype.lib" "${_qt5Widgets_install_prefix}/lib/Qt5FontDatabaseSupport.lib" "${_qt5Widgets_install_prefix}/lib/preprocessor.lib" "${_qt5Widgets_install_prefix}/lib/translator.lib" "${_qt5Widgets_install_prefix}/plugins/platforms/qwindows.lib")
endif (APPLE)

#qt5_use_modules(${PROJECT_NAME} Widgets Gui Network)
###
##################################################################

##################################################################
##################################################################
###  Project Build
###
set(QRC ${CMAKE_SOURCE_DIR}/src/resources.qrc)
#
qt5_wrap_ui(UIS ${FORMS})
qt5_add_resources(RCC ${QRC})
#
#
#


set(BUILD_RESOURCES "${PROJECT_SOURCE_DIR}/src/alloywallet.rc")
#
add_executable(${PROJECT_NAME} ${BUILD_PLATFORM} ${BUILD_RESOURCES} ${SOURCES} ${HEADERS} ${UIS} ${RCC})
#
set_target_properties(${PROJECT_NAME} PROPERTIES COMPILE_DEFINITIONS _GNU_SOURCE)
#
target_compile_definitions(${PROJECT_NAME} PUBLIC -DGIT_REVISION="${GIT_VERSION}")
#
target_include_directories(${PROJECT_NAME} PUBLIC ${PROJECT_SOURCE_DIR}/include ${PROJECT_SOURCE_DIR}/src)

target_link_libraries(${PROJECT_NAME} Qt5::Gui Qt5::Widgets Qt5::Network Qt5::DBus Common)
###
###################################################################
