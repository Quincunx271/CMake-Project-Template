file(GLOB_RECURSE sources "Project/*.hpp" "Project/*.cpp")

add_library(<Project> ${sources})
add_library(<Project>::<Project> ALIAS <Project>)
target_include_directories(<Project>
  PUBLIC
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
    $<INSTALL_INTERFACE:include>
)
target_compile_features(<Project>
  PUBLIC
    cxx_std_14
)
target_link_libraries(<Project> PRIVATE <Project>::Warnings)
