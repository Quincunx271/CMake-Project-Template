cmake_minimum_required(VERSION 3.15)

list(INSERT CMAKE_MODULE_PATH 0 "${CMAKE_CURRENT_SOURCE_DIR}/cmake")

project(Project
  VERSION 0.0.1
  LANGUAGES CXX
  DESCRIPTION "Good Description"
)

include(is_root)
is_root(is_root_project)

set(<PROJECT>_DEVELOPER_DEFAULTS "${is_root_project}" CACHE BOOL "Default all options to developer-friendly values")

option(BUILD_TESTING "Enable testing" ${<PROJECT>_DEVELOPER_DEFAULTS})
option(<PROJECT>_BUILD_DOCS "Build the documentation" OFF)
option(<PROJECT>_TEST_COLOR "Force test color" OFF)
option(<PROJECT>_WARNINGS_AS_ERRORS "Turn on -Werror or equivalent" OFF)

if(BUILD_TESTING)
  enable_testing()
endif()

if(<PROJECT>_DEVELOPER_DEFAULTS)
  include(pmm.cmake)
  pmm(CONAN)
endif()

# Header only library. Changes needed for non-header-only
add_library(<Project> INTERFACE)
add_library(<Project>::<Project> ALIAS <Project>)
target_include_directories(<Project>
  INTERFACE
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
    $<INSTALL_INTERFACE:include>
)
target_compile_features(<Project>
  INTERFACE
    cxx_std_14
)

# Docs
if(<PROJECT>_BUILD_DOCS)
  add_subdirectory(docs)
endif()

# Tests
if(BUILD_TESTING)
  add_subdirectory(tests)
endif()

# Install
include(GNUInstallDirs)
set(INSTALL_CONFIGDIR "${CMAKE_INSTALL_LIBDIR}/cmake/<Project>")

install(TARGETS <Project>
  EXPORT <Project>-Targets
  LIBRARY DESTINATION "${CMAKE_INSTALL_LIBDIR}"
  ARCHIVE DESTINATION "${CMAKE_INSTALL_LIBDIR}"
)

install(EXPORT <Project>-Targets
  FILE <Project>Targets.cmake
  NAMESPACE <Project>::
  DESTINATION "${INSTALL_CONFIGDIR}"
)

install(DIRECTORY include/<Project>/
  DESTINATION "${CMAKE_INSTALL_INCLUDEDIR}/<Project>"
  FILES_MATCHING PATTERN "*.hpp"
)

include(CMakePackageConfigHelpers)

write_basic_package_version_file(
  "${CMAKE_CURRENT_BINARY_DIR}/<Project>ConfigVersion.cmake"
  VERSION "${PROJECT_VERSION}"
  COMPATIBILITY SameMinorVersion
  ARCH_INDEPENDENT
)

install(FILES
  "${CMAKE_CURRENT_BINARY_DIR}/<Project>ConfigVersion.cmake"
  "${CMAKE_CURRENT_SOURCE_DIR}/cmake/<Project>Config.cmake"
  DESTINATION "${INSTALL_CONFIGDIR}"
)

if(<PROJECT>_BUILD_DOCS)
  get_property(docs_dir TARGET docs PROPERTY SPHINX_OUTPUT_DIRECTORY)

  install(DIRECTORY ${docs_dir}
    DESTINATION "${CMAKE_INSTALL_DOCDIR}"
  )
endif()
