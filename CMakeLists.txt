cmake_minimum_required(VERSION 3.10) 

project(Femora)

set(CMAKE_CXX_STANDARD 17)

add_compile_options(-Wall -Wextra -Wpedantic)

option(USE_DOUBLE_PRECISION "Use double instead of float for real numbers" ON)

if (USE_DOUBLE_PRECISION)
    add_compile_definitions(USE_DOUBLE_PRECISION)
endif()

set(FIELD_SRC src/field.cpp)
set(MESH_SRC src/mesh.cpp)
set(MATHCORE_SRC src/vector2d.cpp)
set(GMSH_READER_SRC src/gmsh_reader.cpp)
set(MAIN_SRC src/femora.cpp)

add_library(field ${FIELD_SRC})
add_library(mesh ${MESH_SRC})
add_library(mathcore ${MATHCORE_SRC})
add_library(gmshreader ${GMSH_READER_SRC})

target_include_directories(field PUBLIC ${PROJECT_SOURCE_DIR}/include)
target_include_directories(mesh PUBLIC ${PROJECT_SOURCE_DIR}/include)
target_include_directories(mathcore PUBLIC ${PROJECT_SOURCE_DIR}/include)
target_include_directories(gmshreader PUBLIC ${PROJECT_SOURCE_DIR}/include)

add_executable(femora ${MAIN_SRC})

target_link_libraries(field mesh mathcore)
target_link_libraries(gmshreader mesh mathcore)
target_link_libraries(femora mesh mathcore gmshreader field)


target_include_directories(femora PUBLIC ${PROJECT_SOURCE_DIR}/include)

