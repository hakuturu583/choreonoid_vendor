cmake_minimum_required(VERSION 3.8)
project(choreonoid_vendor)

if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic)
endif()

# find dependencies
find_package(ament_cmake REQUIRED)

macro(build_choreonoid)
  set(cmake_commands)
  set(cmake_configure_args -Wno-dev)
  set(cmake_configure_args -DCMAKE_INSTALL_PREFIX=${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}_install)

  include(ExternalProject)
  externalproject_add(cpp-choreonoid-ext
    GIT_REPOSITORY https://github.com/choreonoid/choreonoid.git
    GIT_TAG master
    TIMEOUT 6000
    ${cmake_commands}
    CMAKE_ARGS
      ${cmake_configure_args}
  )
  install(
    DIRECTORY
      ${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}_install/
    DESTINATION
      ${CMAKE_INSTALL_PREFIX}
  )
  install(
    DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}_install/bin
    DESTINATION lib/choreonoid_vendor
  )
endmacro()

build_choreonoid()
ament_export_include_directories(include)

if(BUILD_TESTING)
  find_package(ament_lint_auto REQUIRED)
  ament_lint_auto_find_test_dependencies()
endif()

ament_package()
