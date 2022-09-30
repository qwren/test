project(test C)
cmake_minimum_required(VERSION 2.6.3)

add_executable (test main.c)

install(TARGETS test DESTINATION ${CMAKE_INSTALL_PREFIX}/bin)

