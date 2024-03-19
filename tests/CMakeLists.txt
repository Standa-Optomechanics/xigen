include(CTest)


if (BUILD_TESTING)

# all test programs

set(XIGEN_ALL_TESTS
	example1.test example2.test example3.test example4.test
	full.test large.test
	code1x.test code2x.test code3.test code5x.test
	def1x.test wiki.test wikiimg.test
	header1.test header2.test header3.test header4.test header5.test header6.test
	header5cs.test header5pas.test header5py.test jni1.test jni2.test
	doc1.test
	fwheader4.test
    )

set (TESTS_FILES
code1x.expected     def1x.expected      header2.expected    header5cs.expected  jni1.expected
code2x.expected     doc1.expected       header3.expected    header5pas.expected jni2.expected
code3.expected      fwheader4.expected  header4.expected    header5py.expected  wiki.expected
code5x.expected     header1.expected    header5.expected    header6.expected    wikiimg.expected
code3.xi    example1.xi example3.xi full.xi     header2.xi  header4.xi  header6.xi  large.xi    wikiimg.xi
doc1.xi     example2.xi example4.xi header1.xi  header3.xi  header5.xi  jni2.xi     wiki.xi
)

# tests targets

add_executable(dummy dummy.cxx)
add_test(NAME dummy COMMAND ${CMAKE_CURRENT_BINARY_DIR}/dummy)

foreach(test ${XIGEN_ALL_TESTS})
    add_test(NAME "${test}" COMMAND ${CMAKE_CURRENT_SOURCE_DIR}/${test})
    set_property(TEST "${test}" PROPERTY ENVIRONMENT "XIGEN=${CMAKE_BINARY_DIR}/src/xigen;srcdir=${CMAKE_CURRENT_SOURCE_DIR}")
endforeach(test ${XIGEN_ALL_TESTS})

# copy test files to the build dir

#foreach(input_test ${TESTS_FILES})
    #set (dest "${PROJECT_BINARY_DIR}/${input_test}")
    #add_custom_command (
        #TARGET     dummy
        #POST_BUILD
        #COMMAND    ${CMAKE_COMMAND}
        #ARGS       -E copy_if_different ${CMAKE_CURRENT_SOURCE_DIR}/${input_test} ${dest}
        #VERBATIM
        #)
#endforeach()

endif()
