cmake_minimum_required(VERSION 3.20)


# -----------------------------------------------------------------------------
project(
        fireworks
        DESCRIPTION "Modern cross-platform graphics engine framework."
        VERSION 0.1
        LANGUAGES C CXX
)

add_custom_target(sdk
        DEPENDS interface runner
)
# -----------------------------------------------------------------------------



# Language related
# -----------------------------------------------------------------------------
set(CMAKE_C_STANDARD 23)
set(CMAKE_CXX_STANDARD 23)

if(NOT CMAKE_BUILD_TYPE)
    set(CMAKE_BUILD_TYPE Debug)
endif()
if (CMAKE_BUILD_TYPE STREQUAL Debug)
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -Wall -Wextra -Wpedantic -Wformat -Wformat-security -Wstack-protector -fstack-protector-strong -Wno-missing-braces -Wno-unused-parameter")
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall -Wextra -Wpedantic -Wformat -Wformat-security -Wstack-protector -fstack-protector-strong -Wno-missing-braces -Wno-unused-parameter")
else ()
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -Wno-stack-protector")
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wno-stack-protector")
endif ()

set(CMAKE_WINDOWS_EXPORT_ALL_SYMBOLS ON)

if(CMAKE_SYSTEM_NAME STREQUAL Windows)
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -Wno-microsoft -Wno-deprecated-declarations")
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wno-microsoft -Wno-deprecated-declarations")
endif()
# -----------------------------------------------------------------------------


# Variables which defines the structure of the output, installed, binaries.
# -----------------------------------------------------------------------------

# The binary and internal paths are hard coded in the program
set(OUT_BINARY_DIRECTORY bin)

set(OUT_ENGINE_DIR engine)
set(OUT_ENGINE_RT_DIR ${OUT_ENGINE_DIR}/runtime)
set(OUT_ENGINE_RT_HEADER_DIR ${OUT_ENGINE_RT_DIR}/include)
set(OUT_ENGINE_UI_DIR ${OUT_ENGINE_DIR}/interface)

set(OUT_PACKAGES_DIR packages)
set(OUT_RUNNER_BIN_DIR ${OUT_BINARY_DIRECTORY})
set(OUT_RUNNER_TOOLS_BIN_DIR ${OUT_RUNTIME_DIRECTORY}/runner)


# -----------------------------------------------------------------------------


# Subcomponents of the sdk
# -----------------------------------------------------------------------------
add_subdirectory(engine/runtime)
add_subdirectory(engine/interface)
add_subdirectory(packages)
add_subdirectory(runner)
# -----------------------------------------------------------------------------


# Basic binary files
# -----------------------------------------------------------------------------
install(FILES license.md DESTINATION .)
install(FILES readme.md DESTINATION .)
# -----------------------------------------------------------------------------