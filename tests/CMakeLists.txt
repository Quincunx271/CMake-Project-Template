find_package(Catch2 REQUIRED)

# Set up warnings / similar flags
set(MSVC_flags /permissive-)
set(Clang_flags -Wall -Wextra -Wpedantic)
set(GNU_flags -Wall -Wextra -Wpedantic)

file(GLOB_RECURSE test_sources CONFIGURE_DEPENDS "*.test.cpp")

add_executable(test.<Project> ${test_sources})
target_link_libraries(test.<Project>
  PRIVATE
    <Project>::<Project>
    Catch2::Catch2
)
target_compile_options(test.<Project>
  PRIVATE
    ${${CMAKE_CXX_COMPILER_ID}_flags}
)
target_compile_features(test.<Project>
  PRIVATE
    cxx_std_14
)

include(Catch)

catch_discover_tests(test.<Project>
  EXTRA_ARGS $<$<BOOL:${<PROJECT>_TEST_COLOR}>:--use-colour=yes>
)
