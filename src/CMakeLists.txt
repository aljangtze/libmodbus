cmake_minimum_required(VERSION 3.20)

project(libmodbus)

set(PROJECT_NAME libmodbus)

set(LIBMODBUS_VERSION_MAJOR 1)
set(LIBMODBUS_VERSION_MINOR 2)
set(LIBMODBUS_VERSION_MICRO 3)
set(LIBMODBUS_VERSION 1.2.3)

set(CMAKE_INCLUDE_CURRENT_DIR ON)


set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

option(BUILD_SHARED_LIBS "Build using shared libraries" OFF)

configure_file("win32/config.h.win32" "${PROJECT_SOURCE_DIR}/config.h")
configure_file("modbus-version.h.in" "${PROJECT_SOURCE_DIR}/modbus-version.h")

add_library(libmodbus
	modbus.c
	modbus.h 
	modbus-data.c 
	modbus-private.h 
	modbus-rtu.c 
	modbus-rtu.h 
	modbus-rtu-private.h 
	modbus-tcp.c 
	modbus-tcp.h 
	modbus-tcp-private.h 
	modbus-version.h
	config.h
	win32/modbus.rc)
if(WIN32)
    target_link_libraries(libmodbus WS2_32)
endif()

target_compile_definitions(libmodbus PRIVATE DLLBUILD)


install(TARGETS libmodbus )
INSTALL(FILES modbus.h modbus-rtu.h modbus-tcp.h DESTINATION include)
