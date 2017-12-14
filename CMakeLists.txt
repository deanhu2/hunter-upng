cmake_minimum_required(VERSION 3.0)
project(upng VERSION 1.0.5)


SET(UPNG_SRC
    ${CMAKE_CURRENT_SOURCE_DIR}/src/upng.c
	${CMAKE_CURRENT_SOURCE_DIR}/src/png2tga.c
)

set(UPNG_HEADER_Files
	${CMAKE_CURRENT_SOURCE_DIR}/src/upng.h
)


add_library(upng ${UPNG_SRC} ${UPNG_HEADER_Files})

target_include_directories(
    upng
    PUBLIC
    $<BUILD_INTERFACE:${CMAKE_CURRENT_LIST_DIR}>
)

# Installation (https://github.com/forexample/package-example) {

set(config_install_dir "lib/cmake/${PROJECT_NAME}")
set(include_install_dir "include")

set(generated_dir ${CMAKE_CURRENT_BINARY_DIR}/generated)

set(version_config ${generated_dir}/${PROJECT_NAME}ConfigVersion.cmake)
set(project_config ${generated_dir}/${PROJECT_NAME}Config.cmake)
set(namespace "${PROJECT_NAME}::")

include(CMakePackageConfigHelpers)

# Use:
#   * PROJECT_VERSION
write_basic_package_version_file(
    "${version_config}" COMPATIBILITY SameMajorVersion
)

# Use:
#   * TARGETS_EXPORT_NAME
#   * PROJECT_NAME
configure_package_config_file(
    "cmake/Config.cmake.in"
    "${project_config}"
    INSTALL_DESTINATION "${config_install_dir}"
)

install(
    TARGETS upng
    EXPORT upngTargets
    LIBRARY DESTINATION lib
    ARCHIVE DESTINATION lib
    RUNTIME DESTINATION bin
    INCLUDES DESTINATION ${include_install_dir}
)


install(
    FILES "${project_config}" "${version_config}"
    DESTINATION "${config_install_dir}"
)

install(
    FILES ${UPNG_HEADER_Files}
    DESTINATION "${include_install_dir}"
)

install(
    EXPORT upngTargets
    NAMESPACE "${namespace}"
    DESTINATION "${config_install_dir}"
)