cmake_minimum_required(VERSION 3.4)
project(testIssue_318)

add_subdirectory(libxlsxwriter)

if(TARGET xlsxwriter)
  message("has the target xlsxwriter")
endif()

if(TARGET testIssue_318)
  message("has the target testIssue_318")
endif()

add_executable(demo src/demo.c)
target_link_libraries(demo xlsxwriter) # links to the target only with exactly one cmake run
# vim: set sw=2 ts=2 et ai:
