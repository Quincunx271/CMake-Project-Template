find_package(Catch2 REQUIRED)

file(GLOB_RECURSE test_sources CONFIGURE_DEPENDS "*.test.cpp")

add_executable(test.<Project> ${test_sources})
target_link_libraries(test.<Project>
  PRIVATE
    <Project>::<Project>
    Catch2::Catch2
    <Project>::Warnings
)
target_compile_options(test.<Project>
  PRIVATE
    ${compile_options}
)
target_link_options(test.<Project>
  PRIVATE
    ${link_options}
)
target_compile_features(test.<Project>
  PRIVATE
    cxx_std_14
)

include(Catch)

catch_discover_tests(test.<Project>
  EXTRA_ARGS $<$<BOOL:${<PROJECT>_TEST_COLOR}>:--use-colour=yes>
)
