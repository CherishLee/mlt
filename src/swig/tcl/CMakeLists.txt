set_source_files_properties(../mlt.i PROPERTIES USE_TARGET_INCLUDE_DIRECTORIES ON CPLUSPLUS ON)

swig_add_library(mlttcl LANGUAGE tcl SOURCES ../mlt.i)
target_link_libraries(mlttcl mlt mlt++)
target_include_directories(mlttcl PRIVATE ${TCL_INCLUDE_PATH})
set_target_properties(mlttcl PROPERTIES OUTPUT_NAME "mlt")
