cmake_minimum_required(VERSION 3.16)
project(LLP2 C)

set(CMAKE_C_STANDARD 11)

set(SRC_DIR ".")
set(PARSER_DIR "${CMAKE_CURRENT_BINARY_DIR}")

find_package(FLEX 2.6 REQUIRED)
find_package(BISON 2.3 REQUIRED)

set(LEXER_OUT "${PARSER_DIR}/lexer.c")
set(PARSER_OUT "${PARSER_DIR}/parser.c")

flex_target(LEXER "${SRC_DIR}/lex.l" "${LEXER_OUT}" DEFINES_FILE "${PARSER_DIR}/lexer.h")
bison_target(PARSER "${SRC_DIR}/bison.y" "${PARSER_OUT}" DEFINES_FILE "${PARSER_DIR}/parser.h" COMPILE_FLAGS "-d -v")
add_flex_bison_dependency(LEXER PARSER)

file(GLOB source_files
        ${CMAKE_CURRENT_SOURCE_DIR}/*.c
        )

add_executable(LLP2 "${source_files}" "${SRC_DIR}/main.c" "${LEXER_OUT}" "${PARSER_OUT}" request_tree.c request_tree.h array_list.c array_list.h)
target_include_directories(LLP2 PRIVATE ${PARSER_DIR} ${SRC_DIR})

target_link_libraries(LLP2 m)