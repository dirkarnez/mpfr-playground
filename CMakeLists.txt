cmake_minimum_required(VERSION 3.14)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

project(mpfr-playground)

add_library(mpfr STATIC IMPORTED)

set_property(
  TARGET mpfr APPEND PROPERTY IMPORTED_CONFIGURATIONS RELEASE
)

set_target_properties(mpfr PROPERTIES
  IMPORTED_LINK_INTERFACE_LANGUAGES_RELEASE "CXX"
  IMPORTED_LOCATION_RELEASE "${MPFR_DIR}/lib/libmpfr.a"
)
  
set_target_properties(mpfr PROPERTIES
  MAP_IMPORTED_CONFIG_MINSIZEREL Release
  MAP_IMPORTED_CONFIG_RELWITHDEBINFO Release
)

target_include_directories(mpfr INTERFACE "${MPFR_DIR}/include")


add_library(gmp STATIC IMPORTED)

set_property(
  TARGET gmp APPEND PROPERTY IMPORTED_CONFIGURATIONS RELEASE
)

set_target_properties(gmp PROPERTIES
  IMPORTED_LINK_INTERFACE_LANGUAGES_RELEASE "CXX"
  IMPORTED_LOCATION_RELEASE "${GMP_DIR}/lib/libgmp.a"
)
  
set_target_properties(gmp PROPERTIES
  MAP_IMPORTED_CONFIG_MINSIZEREL Release
  MAP_IMPORTED_CONFIG_RELWITHDEBINFO Release
)

target_include_directories(gmp INTERFACE "${GMP_DIR}/include")

SET(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} -static")

add_executable(${PROJECT_NAME}
    "src/main.cpp"
)

target_link_libraries(${PROJECT_NAME} mpfr gmp)