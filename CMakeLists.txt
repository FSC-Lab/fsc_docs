cmake_minimum_required(VERSION 3.14.0)
project(fsc_docs)

list(APPEND CMAKE_MODULE_PATH ${CMAKE_SOURCE_DIR}/cmake)

find_package(Python REQUIRED COMPONENTS Interpreter)

find_package(Sphinx REQUIRED)

# HTML output directory
set(SPHINX_HTML_DIR "${CMAKE_BINARY_DIR}/docs/html")

# Building using 'make_docs.py' python script
add_custom_target(
  docs
  COMMAND
    ${SPHINX_EXECUTABLE} -q -b html -c ${CMAKE_CURRENT_SOURCE_DIR}/source -d
    ${CMAKE_CURRENT_BINARY_DIR}/_doctrees ${CMAKE_CURRENT_SOURCE_DIR}/source
    ${CMAKE_CURRENT_BINARY_DIR}/html
  USES_TERMINAL
  COMMENT "Building HTML documentation with Sphinx"
  VERBATIM)
