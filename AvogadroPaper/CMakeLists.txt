project(AvogadroPaper)

cmake_minimum_required(VERSION 2.6)

find_package(LATEX REQUIRED)

add_custom_command(
  OUTPUT  ${CMAKE_CURRENT_BINARY_DIR}/AvogadroPaper.aux
  DEPENDS ${CMAKE_CURRENT_SOURCE_DIR}/AvogadroPaper.tex
  COMMAND ${PDFLATEX_COMPILER}
  ARGS    -interaction=batchmode ${CMAKE_CURRENT_SOURCE_DIR}/AvogadroPaper
  COMMENT "LaTeX first pass"
)

add_custom_command(
  OUTPUT  ${CMAKE_CURRENT_BINARY_DIR}/AvogadroPaper.bbl
  DEPENDS ${CMAKE_CURRENT_BINARY_DIR}/AvogadroPaper.aux
  COMMAND ${BIBTEX_COMPILER}
  ARGS    -terse ${CMAKE_CURRENT_BINARY_DIR}/AvogadroPaper
  COMMENT "BibTeX pass"
)

add_custom_command(
  OUTPUT  ${CMAKE_CURRENT_BINARY_DIR}/AvogadroPaper.pdf
  DEPENDS ${CMAKE_CURRENT_BINARY_DIR}/AvogadroPaper.bbl
  COMMAND ${PDFLATEX_COMPILER}
  ARGS    -interaction=batchmode ${CMAKE_CURRENT_BINARY_DIR}/AvogadroPaper
  COMMENT "LaTeX second pass"
)

add_custom_command(
  OUTPUT  ${CMAKE_CURRENT_BINARY_DIR}/AvogadroPaper.log
  DEPENDS ${CMAKE_CURRENT_BINARY_DIR}/AvogadroPaper.bbl
          ${CMAKE_CURRENT_BINARY_DIR}/AvogadroPaper.pdf
  COMMAND ${PDFLATEX_COMPILER}
  ARGS    -interaction=batchmode ${CMAKE_CURRENT_BINARY_DIR}/AvogadroPaper
  COMMENT "LaTeX third and final pass"
)

add_custom_target(LaTeXPaper ALL echo
  DEPENDS ${CMAKE_CURRENT_BINARY_DIR}/AvogadroPaper.log
)
