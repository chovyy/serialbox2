##===------------------------------------------------------------------------------*- CMake -*-===##
##
##                                   S E R I A L B O X
##
## This file is distributed under terms of BSD license. 
## See LICENSE.txt for more information.
##
##===------------------------------------------------------------------------------------------===##

cmake_minimum_required(VERSION 3.12)

## serialbox_add_gridtools_example
## -------------------------------
##
## Create and install a gridtools example
##
## NAME:STRING=<>     - Name of target and executable
## ARGN:STRINGS=<>    - List of source files
##
function(serialbox_add_gridtools_example NAME)
  add_executable(${NAME} ${ARGN})
  target_link_libraries(${NAME} 
                        SerialboxStatic 
                        GridTools::gridtools
                        ${CMAKE_THREAD_LIBS_INIT})
  target_compile_definitions(${NAME} PUBLIC BOOST_NO_CXX11_DECLTYPE)
  install(TARGETS ${NAME} DESTINATION bin)
  if(SERIALBOX_TESTING)
    serialbox_add_test(TARGET ${NAME} EXECUTABLE ${CMAKE_CURRENT_BINARY_DIR}/${NAME})
  endif(SERIALBOX_TESTING)
endfunction(serialbox_add_gridtools_example)

## serialbox_add_python_example
## ----------------------------
##
## Install a python example
##
## ARGN:STRINGS=<>    - List of source files
##
function(serialbox_add_python_example NAME)
  install(FILES ${NAME} ${ARGN} DESTINATION bin)
endfunction(serialbox_add_python_example)

## Build examples
if(SERIALBOX_ENABLE_C)
  add_subdirectory(c)
endif()

if(SERIALBOX_ENABLE_FORTRAN)
  add_subdirectory(fortran)
endif()

if(SERIALBOX_ENABLE_PYTHON)
  add_subdirectory(python)
endif()

if(SERIALBOX_TESTING_GRIDTOOLS)
  add_subdirectory(gridtools)
endif()
