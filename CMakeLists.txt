cmake_minimum_required(VERSION 3.0.0)

project(BeneOverlay)

set(CMAKE_INCLUDE_CURRENT_DIR ON)
set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTORCC ON)
set(CMAKE_AUTOUIC ON)

find_package(Qt5 COMPONENTS Qml Widgets Quick WebChannel WebSockets REQUIRED)

set(SRC_FILES
  src/dataref.cpp
  src/datarefmanager.cpp
  src/flightsimconnector.cpp
  src/main.cpp
  src/updatemanager.cpp
  src/websocketclientwrapper.cpp
  src/websockettransport.cpp
  )

set(RCC_FILES
  data_resources.qrc
  qml/qml_resources.qrc
  )

set(HDR_FILES
  include/dataref.h
  include/datarefmanager.h
  include/updatemanager.h
  include/flightsimconnector.h
  include/version.h
  include/websocketclientwrapper.h
  include/websockettransport.h
  )

set(IPC_FILES
        fsuipc/IPCuser.c
        )

set(libcurl_INCLUDE_DIR ${CMAKE_CURRENT_SOURCE_DIR}/curl/include)
include_directories(${libcurl_INCLUDE_DIR})

add_executable(BeneOverlay ${SRC_FILES} ${HDR_FILES} ${RCC_FILES} ${IPC_FILES})

target_include_directories(BeneOverlay PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}/include)
target_link_libraries(BeneOverlay Qt5::Widgets Qt5::Qml Qt5::Quick Qt5::WebChannel Qt5::WebSockets ${CMAKE_CURRENT_SOURCE_DIR}/curl/lib/libcurl.dll.a)
target_compile_options(BeneOverlay PRIVATE -Wall -Wextra -Wconversion -Wsign-conversion)

# add_definitions(-DTEST_MODE)

add_custom_command(
        TARGET BeneOverlay POST_BUILD
        COMMAND ${CMAKE_COMMAND} -E copy_if_different $<TARGET_FILE:Qt5::Core> $<TARGET_FILE_DIR:BeneOverlay>
        COMMAND ${CMAKE_COMMAND} -E copy_if_different $<TARGET_FILE:Qt5::Gui> $<TARGET_FILE_DIR:BeneOverlay>
        COMMAND ${CMAKE_COMMAND} -E copy_if_different $<TARGET_FILE:Qt5::Network> $<TARGET_FILE_DIR:BeneOverlay>
        COMMAND ${CMAKE_COMMAND} -E copy_if_different $<TARGET_FILE:Qt5::Qml> $<TARGET_FILE_DIR:BeneOverlay>
        COMMAND ${CMAKE_COMMAND} -E copy_if_different $<TARGET_FILE:Qt5::Quick> $<TARGET_FILE_DIR:BeneOverlay>
        COMMAND ${CMAKE_COMMAND} -E copy_if_different $<TARGET_FILE:Qt5::WebChannel> $<TARGET_FILE_DIR:BeneOverlay>
        COMMAND ${CMAKE_COMMAND} -E copy_if_different $<TARGET_FILE:Qt5::WebSockets> $<TARGET_FILE_DIR:BeneOverlay>
        COMMAND ${CMAKE_COMMAND} -E copy_if_different $<TARGET_FILE:Qt5::Widgets> $<TARGET_FILE_DIR:BeneOverlay>
        COMMAND ${CMAKE_COMMAND} -E copy_if_different ${CMAKE_CURRENT_SOURCE_DIR}/curl/bin/libcurl-x64.dll $<TARGET_FILE_DIR:BeneOverlay>
)
