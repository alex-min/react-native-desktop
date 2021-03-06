
# Copyright (C) 2016, Canonical Ltd.
# All rights reserved.

# This source code is licensed under the BSD-style license found in the
# LICENSE file in the root directory of this source tree. An additional grant
# of patent rights can be found in the PATENTS file in the same directory.

set(CMAKE_INCLUDE_CURRENT_DIR ON)
set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTORCC ON)
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -std=c11")

if ("${CMAKE_BUILD_TYPE}" STREQUAL "Debug")
  ADD_DEFINITIONS(-DRCT_DEV)
endif()

find_package(Qt5Core REQUIRED)
find_package(Qt5Qml REQUIRED)
find_package(Qt5Quick REQUIRED)
find_package(Qt5WebSockets REQUIRED)

set(
  SRC_RUNTIME
  reactplugin.cpp
  valuecoercion.cpp
  blobprovider.cpp
  bridge.cpp
  deviceinfo.cpp
  eventdispatcher.cpp
  sourcecode.cpp
  componentdata.cpp
  moduledata.cpp
  modulemethod.cpp
  propertyhandler.cpp
  networking.cpp
  netinfo.cpp
  timing.cpp
  appstate.cpp
  asynclocalstorage.cpp
  reactitem.cpp
  rootview.cpp
  testmodule.cpp
  attachedproperties.cpp
  uimanager.cpp
  redboxitem.cpp
  exceptionsmanager.cpp
  componentmanagers/slidermanager.cpp
  componentmanagers/viewmanager.cpp
  componentmanagers/rawtextmanager.cpp
  componentmanagers/scrollviewmanager.cpp
  componentmanagers/navigatormanager.cpp
  componentmanagers/activityindicatormanager.cpp
  componentmanagers/buttonmanager.cpp
  componentmanagers/modalmanager.cpp
  componentmanagers/textmanager.cpp
  componentmanagers/textinputmanager.cpp
  componentmanagers/imagemanager.cpp
  componentmanagers/imageloader.cpp
  componentmanagers/pickermanager.cpp
  3rdparty/rnrandombytes.cpp
  layout/flexbox.cpp
  utilities.cpp
  communication/serverconnection.cpp
  communication/executor.cpp
  communication/websocketexecutor.cpp
  communication/iexecutor.h
  websocketmodule.cpp
  ../../../ReactCommon/yoga/yoga/Yoga.c
  ../../../ReactCommon/yoga/yoga/YGEnums.c
  ../../../ReactCommon/yoga/yoga/YGNodeList.c
)

message(STATUS "REACT_NATIVE_DESKTOP_EXTERNAL_MODULES_SRC = ${REACT_NATIVE_DESKTOP_EXTERNAL_MODULES_SRC}")

set (SRC ${SRC_RUNTIME} ${REACT_NATIVE_DESKTOP_EXTERNAL_MODULES_SRC})

set(
  QML
  qml/ReactImage.qml
  qml/ReactView.qml
  qml/ReactNavigator.qml
  qml/ReactScrollView.qml
  qml/ReactRedboxItem.qml
  qml/ReactText.qml
  qml/ReactRawText.qml
  qml/ReactActivityIndicator.qml
  qml/ReactButton.qml
  qml/ReactSlider.qml
  qml/ReactModal.qml
  qml/DevMenu.qml
)

if(DEFINED REACT_BUILD_STATIC_LIB)
add_library(
  react-native STATIC
  ${SRC}
  ${QML}
  react_resources.qrc
)
else()
add_library(
  react-native SHARED
  ${SRC}
  ${QML}
  react_resources.qrc
)
set_target_properties(
  react-native
  PROPERTIES LIBRARY_OUTPUT_DIRECTORY React
)
endif()

set(USED_QT_MODULES Core Qml Quick WebSockets)

qt5_use_modules(react-native ${USED_QT_MODULES})

add_custom_target(
  copy-qmldir ALL
  COMMAND cp ${CMAKE_CURRENT_SOURCE_DIR}/qmldir ${CMAKE_CURRENT_BINARY_DIR}/React
)

configure_file(../../../ubuntu-server.js ../../../ubuntu-server.js COPYONLY)
