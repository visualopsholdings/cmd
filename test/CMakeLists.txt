#   CMakeLists.txt
#
#   Author: Paul Hamilton (paul@visualops.com)
#   Date: 5-May-2023
#
#   Input file for TinyLogo etc for cmake
#
#   This work is licensed under the Creative Commons Attribution 4.0 International License. 
#   To view a copy of this license, visit http://creativecommons.org/licenses/by/4.0/ or 
#   send a letter to Creative Commons, PO Box 1866, Mountain View, CA 94042, USA.
#
#   https://github.com/visualopsholdings/cmd

cmake_minimum_required(VERSION 3.5)

project (flashcmd)
  find_package(Boost COMPONENTS unit_test_framework REQUIRED)
  add_definitions(-DBOOST_ALL_DYN_LINK) 
  enable_testing()

add_executable(CmdTest cmdtest.cpp ../../ringbuffer/ringbuffer.cpp ../cmd.cpp)
  add_definitions(-DRINGBUF_SIZE_MINUS_1=8 -DCMD_SIZE=20) 
  target_link_libraries(CmdTest ${Boost_UNIT_TEST_FRAMEWORK_LIBRARY})

add_test(CmdTest CmdTest)
