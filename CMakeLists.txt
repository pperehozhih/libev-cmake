# Copyright (C) 2007-2013 LuaDist.
# Created by Peter Drahoš, Peter Kapec
# Redistribution and use of this file is allowed according to the terms of the MIT license.
# For details see the COPYRIGHT file distributed with LuaDist.
# Please note that the package source code is licensed under its own license.

project ( libev C )

if(APPLE)
  set(CMAKE_MACOSX_RPATH 1)
endif()


cmake_minimum_required ( VERSION 2.8 )
include ( "cmake/configure.cmake" )

configure_file (${CMAKE_CURRENT_SOURCE_DIR}/config.h.cmake ${CMAKE_CURRENT_BINARY_DIR}/config.h)

set (EV_SRC
  ${CMAKE_CURRENT_SOURCE_DIR}/libev/ev.c
  ${CMAKE_CURRENT_SOURCE_DIR}/libev/event.c
)

set(EV_LIBS "")

if(WIN32)
  list(APPEND EV_LIBS Ws2_32)
  add_definitions(-DEV_USE_SELECT -DHAVE_SELECT -DHAVE_SYS_SELECT_H)
endif()

add_library(ev SHARED ${EV_SRC})
target_link_libraries (ev ${EV_LIBS})
target_include_directories(ev PUBLIC ${CMAKE_CURRENT_BINARY_DIR})
target_include_directories(ev PUBLIC libev)

add_library(ev_static STATIC ${EV_SRC})
target_link_libraries (ev_static ${EV_LIBS})
target_include_directories(ev_static PUBLIC ${CMAKE_CURRENT_BINARY_DIR})
target_include_directories(ev_static PUBLIC libev)

install(FILES 
    ${CMAKE_CURRENT_SOURCE_DIR}/libev/ev++.h
    ${CMAKE_CURRENT_SOURCE_DIR}/libev/ev.h
    ${CMAKE_CURRENT_SOURCE_DIR}/libev/event.h
    DESTINATION include
)

install(TARGETS ev_static
  RUNTIME DESTINATION bin
  LIBRARY DESTINATION lib
  ARCHIVE DESTINATION lib
)

install(TARGETS ev
  RUNTIME DESTINATION bin
  LIBRARY DESTINATION lib
  ARCHIVE DESTINATION lib
)
