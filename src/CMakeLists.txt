# Add the executable and explicitly list source files
add_executable(wiki
    main.cpp
    utils.cpp
)

# Include directories for headers
target_include_directories(wiki PRIVATE ${CMAKE_CURRENT_SOURCE_DIR})