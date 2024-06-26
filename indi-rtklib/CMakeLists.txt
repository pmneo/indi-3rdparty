########### NexStar Evolution INDI driver ##############
PROJECT(indi_rtklib C CXX)
cmake_minimum_required(VERSION 3.16)

include(GNUInstallDirs)

LIST(APPEND CMAKE_MODULE_PATH "${CMAKE_CURRENT_SOURCE_DIR}/cmake_modules/")
LIST(APPEND CMAKE_MODULE_PATH "${CMAKE_CURRENT_SOURCE_DIR}/../cmake_modules/")

set(BIN_INSTALL_DIR "${CMAKE_INSTALL_PREFIX}/bin")
set(DATA_INSTALL_DIR "${CMAKE_INSTALL_PREFIX}/share/indi/")
set(INDI_DATA_DIR "${CMAKE_INSTALL_PREFIX}/share/indi")

find_package(INDI REQUIRED)
find_package(Nova REQUIRED)
find_package(ZLIB REQUIRED)
find_package(Threads REQUIRED)

set(RTKLIB_VERSION_MAJOR 0)
set(RTKLIB_VERSION_MINOR 1)

configure_file(${CMAKE_CURRENT_SOURCE_DIR}/config.h.cmake ${CMAKE_CURRENT_BINARY_DIR}/config.h )
configure_file(${CMAKE_CURRENT_SOURCE_DIR}/indi_rtklib.xml.cmake ${CMAKE_CURRENT_BINARY_DIR}/indi_rtklib.xml )

include_directories( ${CMAKE_CURRENT_BINARY_DIR})
include_directories( ${CMAKE_CURRENT_SOURCE_DIR})
include_directories( ${INDI_INCLUDE_DIR})
include_directories( ${NOVA_INCLUDE_DIR})

include(CMakeCommon)

add_executable(indi_rtklib rtklib_driver.cpp rtkrcv_parser.c)
target_link_libraries(indi_rtklib ${INDI_LIBRARIES} ${NOVA_LIBRARIES} ${CMAKE_THREAD_LIBS_INIT})
install(TARGETS indi_rtklib RUNTIME DESTINATION bin )

install( FILES  ${CMAKE_CURRENT_BINARY_DIR}/indi_rtklib.xml DESTINATION ${INDI_DATA_DIR})
