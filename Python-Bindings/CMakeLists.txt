cmake_minimum_required(VERSION 3.10)
project(topologic)

set(CMAKE_CXX_STANDARD 17)
IF(MSVC)  
  find_package(PythonLibs REQUIRED)
ENDIF(MSVC)

list(APPEND CMAKE_PREFIX_PATH "./pybind")
find_package(pybind11 REQUIRED)

include_directories(${PYTHON_INCLUDE_DIRS})
IF(MSVC)
  include_directories(../TopologicCore/include)
include_directories(../TopologicCore/include)
ELSE(MSVC)
  include_directories(/usr/include/TopologicCore)
  include_directories(/usr/include/opencascade)
  include_directories(/usr/local/include/TopologicCore)
  include_directories(/usr/local/include/opencascade)
ENDIF(MSVC)

include_directories(./include)

# This is a hardcoded path as we expect OpenCASCADE to be exactly
# in this directory. Revisit if this changes!
IF(MSVC)
  include_directories(OCC_INCLUDE_DIR)
  SET(CUSTOM_LIB_PATH OCC_LIB_DIR)
ENDIF(MSVC)

#add_library(TopologicCore SHARED IMPORTED)
set(ADDITIONAL_LIBRARY_DEPENDENCIES
    "TKernel;TKMath"
)

IF(MSVC)
  SET_PROPERTY(TARGET TopologicCore PROPERTY IMPORTED_LOCATION ${CMAKE_CURRENT_LIST_DIR}/../output/${CMAKE_GENERATOR_PLATFORM}/${CMAKE_BUILD_TYPE}/TopologicCore.dll)
  SET_PROPERTY(TARGET TopologicCore PROPERTY IMPORTED_IMPLIB ${CMAKE_CURRENT_LIST_DIR}/../output/${CMAKE_GENERATOR_PLATFORM}/${CMAKE_BUILD_TYPE}/TopologicCore.lib)
else(MSVC)
  find_library(TOPOLOGIC_CORE TopologicCore)
  SET_PROPERTY(TARGET TopologicCore PROPERTY IMPORTED_LOCATION ${TOPOLOGIC_CORE})
ENDIF(MSVC)

set(SRC_FILES 
  ./src/TopologicalQuery.cppwg.cpp
  ./src/Topology.cppwg.cpp
  ./src/Vertex.cppwg.cpp
  ./src/Edge.cppwg.cpp
  ./src/Wire.cppwg.cpp
  ./src/Face.cppwg.cpp
  ./src/Shell.cppwg.cpp
  ./src/Cell.cppwg.cpp
  ./src/CellComplex.cppwg.cpp
  ./src/Cluster.cppwg.cpp
  ./src/Aperture.cppwg.cpp
  ./src/Graph.cppwg.cpp
  ./src/Attribute.cppwg.cpp
  ./src/Dictionary.cppwg.cpp
  ./src/ContentManager.cppwg.cpp
  ./src/Context.cppwg.cpp
  ./src/IntAttribute.cppwg.cpp
  ./src/StringAttribute.cppwg.cpp
  ./src/DoubleAttribute.cppwg.cpp
  ./src/ListAttribute.cppwg.cpp
  ./src/VertexUtility.Binding.cpp
  ./src/EdgeUtility.Binding.cpp
  ./src/WireUtility.Binding.cpp
  ./src/ShellUtility.Binding.cpp
  ./src/CellUtility.Binding.cpp
  ./src/TopologyUtility.Binding.cpp
  ./src/FaceUtility.Binding.cpp
  ./src/GlobalCluster.Binding.cpp
  ./src/main.cpp
)

pybind11_add_module(${PROJECT_NAME} SHARED ${SRC_FILES})
target_link_directories(${PROJECT_NAME} PUBLIC ${CUSTOM_LIB_PATH})
target_link_libraries(${PROJECT_NAME} PRIVATE TopologicCore ${ADDITIONAL_LIBRARY_DEPENDENCIES})

use_props(${PROJECT_NAME} "${CMAKE_CONFIGURATION_TYPES}" "${DEFAULT_CXX_PROPS}")
set(ROOT_NAMESPACE TopologicCore)

################################################################################
# Output directory
################################################################################
if("${CMAKE_VS_PLATFORM_NAME}" STREQUAL "Any CPU")
    set_target_properties(${PROJECT_NAME} PROPERTIES
        OUTPUT_DIRECTORY_DEBUG   "${CMAKE_SOURCE_DIR}/output/${CMAKE_VS_PLATFORM_NAME}/$<CONFIG>/"
        OUTPUT_DIRECTORY_RELEASE "${CMAKE_SOURCE_DIR}/output/${CMAKE_VS_PLATFORM_NAME}/$<CONFIG>/"
    )
elseif("${CMAKE_VS_PLATFORM_NAME}" STREQUAL "x64")
    set_target_properties(${PROJECT_NAME} PROPERTIES
        OUTPUT_DIRECTORY_DEBUG   "${CMAKE_SOURCE_DIR}/output/${CMAKE_VS_PLATFORM_NAME}/$<CONFIG>/"
        OUTPUT_DIRECTORY_RELEASE "${CMAKE_SOURCE_DIR}/output/${CMAKE_VS_PLATFORM_NAME}/$<CONFIG>/"
    )
elseif("${CMAKE_VS_PLATFORM_NAME}" STREQUAL "x86")
    set_target_properties(${PROJECT_NAME} PROPERTIES
        OUTPUT_DIRECTORY_DEBUG   "${CMAKE_SOURCE_DIR}/output/${CMAKE_VS_PLATFORM_NAME}/$<CONFIG>/"
        OUTPUT_DIRECTORY_RELEASE "${CMAKE_SOURCE_DIR}/output/${CMAKE_VS_PLATFORM_NAME}/$<CONFIG>/"
    )
endif()
if("${CMAKE_VS_PLATFORM_NAME}" STREQUAL "Any CPU")
    set_target_properties(${PROJECT_NAME} PROPERTIES
        INTERPROCEDURAL_OPTIMIZATION_RELEASE "TRUE"
    )
elseif("${CMAKE_VS_PLATFORM_NAME}" STREQUAL "x64")
    set_target_properties(${PROJECT_NAME} PROPERTIES
        INTERPROCEDURAL_OPTIMIZATION_RELEASE "TRUE"
    )
elseif("${CMAKE_VS_PLATFORM_NAME}" STREQUAL "x86")
    set_target_properties(${PROJECT_NAME} PROPERTIES
        INTERPROCEDURAL_OPTIMIZATION_RELEASE "TRUE"
    )
endif()
