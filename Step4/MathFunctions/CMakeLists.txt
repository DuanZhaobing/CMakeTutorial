add_library(MathFunctions MathFunctions.cxx)

# TODO 1: State that anybody linking to MathFunctions needs to include the
# current source directory, while MathFunctions itself doesn't.
# Hint: Use target_include_directories with the INTERFACE keyword
# PUBLIC 本目标需要，依赖这个目标的其他目标也需要
# INTERFACE 本目标不需要，依赖这个目标的其他目标需要
# PRIVATE 本目标不需要，依赖这个目标的其他目标也不需要
target_include_directories(MathFunctions 
                           INTERFACE ${CMAKE_CURRENT_SOURCE_DIR}
                          )

# should we use our own math functions
option(USE_MYMATH "Use tutorial provided math implementation" ON)
message(STATUS "OUT --- USE_MYMATH -- ${USE_MYMATH}")

if (USE_MYMATH)
  target_compile_definitions(MathFunctions PRIVATE "USE_MYMATH")

  # library that just does sqrt
  add_library(SqrtLibrary STATIC
              mysqrt.cxx
              )

  # TODO 6: Link SqrtLibrary to tutorial_compiler_flags
  target_link_libraries(SqrtLibrary PUBLIC tutorial_compiler_flags)
  message(STATUS "USE_MYMATH -- ${USE_MYMATH}")
  target_link_libraries(MathFunctions PRIVATE SqrtLibrary)

endif()

# TODO 7: Link MathFunctions to tutorial_compiler_flags
target_link_libraries(MathFunctions PUBLIC tutorial_compiler_flags)
