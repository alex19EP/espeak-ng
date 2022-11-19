add_subdirectory(ucd-tools)
add_subdirectory(speechPlayer)
add_subdirectory(libespeak-ng)

add_executable(espeak-ng espeak-ng.c)
target_link_libraries(
  espeak-ng PRIVATE libespeak-ng libespeak-ng-config
)
add_custom_target(
  speak-ng ALL
  ${CMAKE_COMMAND} -E create_symlink espeak-ng ${CMAKE_CURRENT_BINARY_DIR}/speak-ng
  COMMENT "Link espeak-ng to speak-ng"
  DEPENDS espeak-ng
)
