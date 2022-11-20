include(CTest)

list(APPEND _binary_tests)

macro(compiled_test _test_name)
  add_executable(test_${_test_name} ${_test_name}.c)
  target_link_libraries(test_${_test_name} PRIVATE libespeak-ng libespeak-ng-config)
  target_include_directories(
    test_${_test_name} PRIVATE
    $<TARGET_PROPERTY:libespeak-ng,SOURCE_DIR>
    $<TARGET_PROPERTY:libespeak-ng,SOURCE_DIR>/include/compat
  )
  add_dependencies(test_${_test_name} data)
  add_test(
    NAME ${_test_name}
    COMMAND env ESPEAK_DATA_PATH=${DATA_DIST_ROOT} $<TARGET_FILE:test_${_test_name}>
  )
  list(APPEND _binary_tests test_${_test_name})
endmacro(compiled_test)

macro(shell_test _test_name)
  add_test(
    NAME ${_test_name}
    COMMAND env ESPEAK_DATA_PATH=${DATA_DIST_ROOT} ESPEAK_BIN=$<TARGET_FILE:espeak-ng> ${CMAKE_CURRENT_SOURCE_DIR}/${_test_name}.test
  )
endmacro(shell_test)

compiled_test(api)
compiled_test(encoding)
compiled_test(ieee80)
compiled_test(readclause)

shell_test(bom)
shell_test(non-executable-files-with-executable-bit)

shell_test(cmd_options)
shell_test(dictionary)
shell_test(language-numbers-cardinal)
shell_test(language-numbers-ordinal)
shell_test(language-phonemes)
shell_test(language-pronunciation)
shell_test(language-replace)
shell_test(ssml)
shell_test(translate)
shell_test(variants)
shell_test(voices)

# shell_test(windows-data)
# shell_test(windows-installer)

shell_test(klatt)
shell_test(mbrola)

add_custom_target(tests DEPENDS ${_binary_tests})
