cmake_minimum_required(VERSION 2.8)
project(synthlib2parser)

set (CMAKE_CXX_STANDARD 11)

find_package(BISON REQUIRED)
find_package(FLEX REQUIRED)

BISON_TARGET(SynthLib2Parser ${CMAKE_CURRENT_SOURCE_DIR}/src/SynthLib2Parser.y ${CMAKE_CURRENT_BINARY_DIR}/SynthLib2Parser.tab.cpp DEFINES_FILE ${CMAKE_CURRENT_BINARY_DIR}/SynthLib2Parser.tab.h)
FLEX_TARGET(SynthLib2Scanner ${CMAKE_CURRENT_SOURCE_DIR}/src/SynthLib2Lexer.l ${CMAKE_CURRENT_BINARY_DIR}/SynthLib2Lexer.cpp)
ADD_FLEX_BISON_DEPENDENCY(SynthLib2Scanner SynthLib2Parser)

#BISON_TARGET(MyParser parser.y ${CMAKE_CURRENT_BINARY_DIR}/parser.cpp DEFINES_FILE ${CMAKE_CURRENT_BINARY_DIR}/parser.h)

add_library(synthlib2parser STATIC
        ${BISON_SynthLib2Parser_OUTPUTS}
        ${FLEX_SynthLib2Scanner_OUTPUTS}
        ${CMAKE_CURRENT_SOURCE_DIR}/src/LogicSymbols.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/src/PrintVisitor.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/src/SymbolTable.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/src/SymtabBuilder.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/src/SynthLib2ParserAST.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/src/SynthLib2ParserTester.cpp
        )
target_include_directories(synthlib2parser PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}/src/include)