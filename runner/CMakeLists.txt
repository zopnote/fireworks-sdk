
set(BIN_APP_DIR ${CMAKE_BINARY_DIR}/runner)

file(COPY ${CMAKE_CURRENT_SOURCE_DIR}/bin DESTINATION ${BIN_APP_DIR})
file(COPY ${CMAKE_CURRENT_SOURCE_DIR}/lib DESTINATION ${BIN_APP_DIR})
file(COPY ${CMAKE_CURRENT_SOURCE_DIR}/pubspec.yaml DESTINATION ${BIN_APP_DIR})

# Dart entry points to start the server and manage the web app at runtime
# -----------------------------------------------------------------------------


find_program(DART_EXE dart)

if(NOT DART_EXE)
    message(FATAL_ERROR "Dart executable not found in path.")
endif ()

if(CMAKE_SYSTEM_NAME STREQUAL Windows)
    set(EXECUTABLE_ENDING .exe)
else ()
    set(EXECUTABLE_ENDING)
endif ()

set(OUTPUT_BINARY ${BIN_APP_DIR}/fireworks${EXECUTABLE_ENDING})
set(ENTRY_POINT ${BIN_APP_DIR}/bin/fireworks.dart)

add_custom_command(
        OUTPUT ${OUTPUT_BINARY}
        COMMAND ${DART_EXE} compile exe ${ENTRY_POINT} -o ${OUTPUT_BINARY}
        WORKING_DIRECTORY ${BIN_APP_DIR}
        COMMENT "Compile interface dart script ${ENTRY_POINT}."
        VERBATIM
)

file(GLOB_RECURSE BIN_SOURCE_FILES ${CMAKE_CURRENT_SOURCE_DIR}/bin/*)
file(GLOB_RECURSE LIB_SOURCE_FILES ${CMAKE_CURRENT_SOURCE_DIR}/lib/*)
add_custom_target(runner
        DEPENDS ${OUTPUT_BINARY}
        SOURCES ${BIN_SOURCE_FILES} ${LIB_SOURCE_FILES}
)

install(FILES ${OUTPUT_BINARY} DESTINATION ${OUT_BINARY_DIRECTORY})
# -----------------------------------------------------------------------------
