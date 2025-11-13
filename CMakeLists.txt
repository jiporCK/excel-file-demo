cmake_minimum_required(VERSION 3.14)    # Require CMake 3.14 or newer
project(ExcelTableApp)                  # Project name
set(CMAKE_CXX_STANDARD 17)              # Use C++17 standard

include(FetchContent)                   # Enable FetchContent for dependencies

# Get xlnt library (Excel handling)
FetchContent_Declare(
  xlnt
  GIT_REPOSITORY https://github.com/tfussell/xlnt.git
  GIT_TAG master
)
FetchContent_MakeAvailable(xlnt)

# Get tabulate library (table formatting)
FetchContent_Declare(
  tabulate
  GIT_REPOSITORY https://github.com/p-ranav/tabulate.git
  GIT_TAG master
)
FetchContent_MakeAvailable(tabulate)

# Build executable from source files
add_executable(ExcelTableApp main.cpp src/Student.cpp src/ExcelUtils.cpp src/MenuUtils.cpp)

# Include header directory
target_include_directories(ExcelTableApp PRIVATE include)

# Link libraries to the app
target_link_libraries(ExcelTableApp PRIVATE xlnt tabulate)

# Copy xlnt.dll after build (Windows)
add_custom_command(TARGET ExcelTableApp POST_BUILD
  COMMAND ${CMAKE_COMMAND} -E copy_if_different
    "${CMAKE_SOURCE_DIR}/build/_deps/xlnt-build/source/Release/xlnt.dll"
    $<TARGET_FILE_DIR:ExcelTableApp>)
