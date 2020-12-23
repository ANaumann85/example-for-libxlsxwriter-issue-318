cmake_minimum_required(VERSION 3.4)
project(testIssue_318)

add_subdirectory(libxlsxwriter)

if(TARGET xlsxwriter)
  message("has the target xlsxwriter")
endif()

# vim: set sw=2 ts=2 et ai:
