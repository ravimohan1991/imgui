# For adding multiple files
file(GLOB ImGuiSource *.cpp)

# Building the project (STATIC or DYNAMIC (SHARED))
if (BUILD_SHARED_LIBS)
    add_library(ImGui SHARED ${ImGuiSource})
else()
    add_library(ImGui STATIC ${ImGuiSource})
endif()


# INTERFACE (Something that Consumers require but Producers don't)
# Since I am a library coder here, I am the Producer
# Rest of the Engine is Consumer here

target_include_directories(ImGui
    PRIVATE
        # where the library itself will look for its internal headers
        ${CMAKE_CURRENT_SOURCE_DIR}
    PUBLIC
        # where top-level project will look for the library's public headers
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>
        # where external projects will look for the library's public headers
        $<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}>
)

target_compile_definitions(Glad PUBLIC Glad)
