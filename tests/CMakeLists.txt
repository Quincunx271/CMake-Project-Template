find_package(Catch2 REQUIRED)

# Set up warnings / similar flags
set(werr ${<PROJECT>_WARNINGS_AS_ERRORS})
set(compile_options)
set(link_options)

if(CMAKE_CXX_COMPILER_ID STREQUAL MSVC)
  list(APPEND compile_options
    /permissive- /W4
    $<$<BOOL:${werr}>:/WX>
  )
elseif(CMAKE_CXX_COMPILER_ID STREQUAL GNU)
  list(APPEND compile_options
    -Wall -Wextra -Wpedantic
    -fsanitize=address
    -fsanitize=undefined
    $<$<BOOL:${werr}>:-Werror>
  )
  list(APPEND link_options
    -fsanitize=address
    -fsanitize=undefined
  )
elseif(CMAKE_CXX_COMPILER_ID STREQUAL Clang)
  list(APPEND compile_options
    -Wall -Wextra -Wpedantic
    -fsanitize=address
    -fsanitize=undefined
    $<$<BOOL:${werr}>:-Werror>
  )
  list(APPEND link_options
    -fsanitize=address
    -fsanitize=undefined
  )
endif()

file(GLOB_RECURSE test_sources CONFIGURE_DEPENDS "*.test.cpp")

add_executable(test.<Project> ${test_sources})
target_link_libraries(test.<Project>
  PRIVATE
    <Project>::<Project>
    Catch2::Catch2
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
