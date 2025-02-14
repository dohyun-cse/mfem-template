cmake_minimum_required(VERSION 3.8)
project(MyApp)

# Ensure C++14 or newer
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)  # Use strict C++ standard
# Ensure MPI is found before including MFEM
find_package(MPI REQUIRED)
set(CMAKE_CXX_COMPILER ${MPI_CXX_COMPILER})  # Set MPI compiler if needed

# Use the MFEM build directory directly
set(MFEM_DIR "${CMAKE_SOURCE_DIR}/../mfem/build") 

# Include MFEM
include("${MFEM_DIR}/MFEMConfig.cmake")

# Create executable
add_executable(myapp myapp.cpp)

# Link with MFEM
target_link_libraries(myapp PRIVATE mfem)

add_custom_target(dataclean
  COMMAND ${CMAKE_COMMAND} -E echo "Cleaning up log files and data files..."
  COMMAND find ${CMAKE_BINARY_DIR} -type f -name "*.mesh" -delete
  COMMAND find ${CMAKE_BINARY_DIR} -type f -name "*.gf" -delete
  COMMAND ${CMAKE_COMMAND} -E rm -rf ${CMAKE_BINARY_DIR}/ParaView
  COMMAND ${CMAKE_COMMAND} -E rm -rf ${CMAKE_BINARY_DIR}/results
  COMMAND ${CMAKE_COMMAND} -E rm -rf ${CMAKE_BINARY_DIR}/data
)
