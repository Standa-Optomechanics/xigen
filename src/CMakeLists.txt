# set fPIC for static and shared libs

SET (CMAKE_POSITION_INDEPENDENT_CODE ON)

SET (SOURCES_STATIC
     driver.cc
     generator.cc
     locale.cc
     xigen.cc
)


if (NOT MSVC)

    # Try to find BISON and FLEX
    find_package(BISON)
    find_package(FLEX)

    BISON_TARGET(Parser ${CMAKE_SOURCE_DIR}/src/parser.yy ${CMAKE_CURRENT_BINARY_DIR}/parser.cc)

    FLEX_TARGET(Scanner ${CMAKE_SOURCE_DIR}/src/scanner.ll ${CMAKE_CURRENT_BINARY_DIR}/lexer.cc)

    ADD_FLEX_BISON_DEPENDENCY(Scanner Parser)

    add_executable(xigen
                   ${SOURCES_STATIC}
                   ${FLEX_Scanner_OUTPUTS}
                   ${BISON_Parser_OUTPUTS}
    )

    target_include_directories(xigen PRIVATE ${CMAKE_CURRENT_BINARY_DIR})
else()
    add_executable(xigen
                   ${SOURCES_STATIC}
                   ${CMAKE_SOURCE_DIR}/src/compat/getopt_long.c
                   ${CMAKE_SOURCE_DIR}/src/parser.cc
                   ${CMAKE_SOURCE_DIR}/src/scanner.cc
    )
    target_include_directories(xigen PRIVATE ${CMAKE_SOURCE_DIR}/src/compat)

    # Generate parser and scanner by direct invoking flex and bison
    set_source_files_properties(${CMAKE_SOURCE_DIR}/src/parser.cc ${CMAKE_SOURCE_DIR}/src/scanner.cc
                                PROPERTIES GENERATED TRUE)
    add_custom_command(TARGET xigen PRE_BUILD
                       COMMAND c:/cygwin/bin/bash.exe --login -c "cd  `cygpath %27${CMAKE_SOURCE_DIR}/src%27` && flex -i -+ -oscanner.cc scanner.ll && bison -y -d -v -ra -S lalr1.cc -o parser.cc parser.yy"
    )
endif()

target_include_directories(xigen PRIVATE ${CMAKE_SOURCE_DIR}/src)

# Tune warnings
if(MSVC)
    target_compile_options(xigen PRIVATE /W3 /wd"4267" /wd"4244" /wd"4390" /wd"4800" /WX  /D"_CRT_SECURE_NO_WARNINGS" 
                           /D"_CRT_SECURE_NO_DEPRECATE" /D"_CRT_NONSTDC_NO_DEPRECATE"
                           /D"_USING_V110_SDK71_" /D"_UNICODE" /D"UNICODE" /errorReport:prompt)
else()
    target_compile_options(xigen PRIVATE -Wno-deprecated-declarations -Wno-tautological-compare)
endif()

install(TARGETS xigen DESTINATION bin)
