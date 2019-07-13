cmake_minimum_required (VERSION 3.14)
# Name your project. Can be anything.
project (SampleProject)

# Set binary output directories. Changing these paths will require you adjusting your resource path configuration!
set (CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)
set (CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)
set (CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)
set (CMAKE_POSITION_INDEPENDENT_CODE ON)

set (BUILD_SHARED_LIBS ON CACHE BOOLEAN "" FORCE)

if(NOT CMAKE_CXX_STANDARD)
    set (CMAKE_CXX_STANDARD 17)
    message(WARNING "Project top c++ standard has not been specified, setting to: ${CMAKE_CXX_STANDARD}")
else()
    message("Project top c++ standard: ${CMAKE_CXX_STANDARD}")
endif()

set (CMAKE_CXX_STANDARD_REQUIRED ON)

# Include dependencies in the build
add_subdirectory(dep)

# Include rest of the project in the build
add_subdirectory(GameCore)
add_subdirectory(GameApplication)
if (NOT WEB)
    add_subdirectory(EpnSampleNativePlugin)
    add_target_csharp(EpmSampleManagedPlugin EpmSampleManagedPlugin/EpmSampleManagedPlugin.csproj)
endif()
if (NOT MSVC)
    add_msbuild_target(TARGET NugetRestore2 EXCLUDE_FROM_ALL AUTORUN_AT_CONFIGURE ARGS
        ${CMAKE_CURRENT_SOURCE_DIR}/ManagedTargets.sln /t:restore /m /consoleloggerparameters:ErrorsOnly)
endif ()
