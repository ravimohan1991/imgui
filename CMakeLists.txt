#[[
    Abstractions and Models are NOT at WAR!
                                            - Cobwoy's Talisman
    But Abstractions don't care and Models can't understand!!
                                            - Lul, Practicality
 ]]

# For adding multiple files
file(GLOB ImGuiSource *.cpp)
list(APPEND ImGuiHeaders imgui.h backends/imgui_impl_vulkan.h backends/imgui_impl_sdl.h imgui_internal.h)

# For vulkan support
list(APPEND ImGuiSource backends/imgui_impl_vulkan.cpp backends/imgui_impl_sdl.cpp)

# Set the project name and version
project(ImGUI VERSION 1.0) # Versioning shall change to something more appropriate

#set(UCRT "")
#find_library(UCRT ucrtd WINDOWS_KITS_BIN_PATH)
#message(STATUS "-------------> ${UCRT}")

#set(CMAKE_BUILD_TYPE DEBUG)

#set(CMAKE_C_FLAGS "/MD")
#set(CMAKE_C_FLAGS_DEBUG "${CMAKE_C_FLAGS_DEBUG} /MD")
#set(CMAKE_C_FLAGS_RELEASE "/MD")
#set(CMAKE_CXX_FLAGS "/MD")
#set(CMAKE_CXX_FLAGS_DEBUG "${CMAKE_CXX_FLAGS_DEBUG} /MD")
#set(CMAKE_CXX_FLAGS_RELEASE "/MD")

#add_definitions(/MD)

# Building the project (STATIC or DYNAMIC (SHARED))
# Thanks to SDL and Microsoft, we shall drop dll support for Windows
if (BUILD_SHARED_LIBS AND NOT MSVC)
    add_library(ImGUI SHARED ${ImGuiSource} ${ImGuiHeaders})
else()
    add_library(ImGUI STATIC ${ImGuiSource} ${ImGuiHeaders})
endif()


# INTERFACE (Something that Consumers require but Producers don't)
# Since I am a library coder here, I am the Producer
# Rest of the Engine is Consumer here

target_include_directories(ImGUI
    PRIVATE
        # where the library itself will look for its internal headers
        ${CMAKE_CURRENT_SOURCE_DIR}
        ${CMAKE_CURRENT_SOURCE_DIR}/backends
        ${Vulkan_INCLUDE_DIR}
        ${CMAKE_CURRENT_SOURCE_DIR}/../SDL/include
    PUBLIC
        # where top-level project will look for the library's public headers
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>
		$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/backends>
        # where external projects will look for the library's public headers
        $<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}>
)

target_compile_definitions(ImGUI PUBLIC ImGUI)
target_link_libraries(ImGUI PUBLIC ${Vulkan_LIBRARY})

if(NOT MSVC)
if (BUILD_SHARED_LIBS)
   target_link_libraries(ImGUI PUBLIC SDL2)
else()
    target_link_libraries(ImGUI PUBLIC SDL2-static)
endif()
endif()

#foreach(flag_var
#        CMAKE_C_FLAGS CMAKE_C_FLAGS_DEBUG CMAKE_C_FLAGS_RELEASE
#        CMAKE_C_FLAGS_MINSIZEREL CMAKE_C_FLAGS_RELWITHDEBINFO)
#      Message(STATUS "${${flag_var}}")
#endforeach()
#message(STATUS "${CMAKE_C_FLAGS}")

#add_library(CRT "${CMAKE_CURRENT_SOURCE_DIR}/ucrtbased.dll")

#unset(${UCRT})
#find_library(UCRT ucrtd "C:/Program Files (x86)/Windows Kits/10/Lib/10.0.22621.0/ucrt/x64/ucrtd.lib")
#message(STATUS "${ucrtd}")

#set(DLL_LINK_LIBS
#    ${UCRT} ucrtd.lib) #libucrtd.lib ucrt.lib ucrt.osmode.lib ucrt.osmode_permissive.lib ucertd.lib ucrtd.osmode.lib
#ucrtd.osmode_permissive.lib)

#target_link_libraries(ImGUI PRIVATE ${UCRT})

#[[
 *                                          /\
 *                                         / /
 *                                      /\| |
 *                                      | | |/\
 *                                      | | / /
 *                                      | `  /
 *                                      `\  (___
 *                                     _.->  ,-.-.
 *                                  _.'      |  \ \
 *                                 /    _____| 0 |0\
 *                                |    /`    `^-.\.-'`-._
 *                                |   |                  `-._
 *                                |   :                      `.
 *                                \    `._     `-.__         O.'
 *         _.--,                   \     `._     __.^--._O_..-'
 *        `---, `.                  `\     /` ` `
 *             `\ `,                  `\   |
 *              |   :                   ;  |
 *              /    `.              ___|__|___
 *             /       `.           (          )
 *            /    `---.:____...---' `--------`.
 *           /        (         `.      __      `.
 *          |          `---------' _   /  \       \
 *          |    .-.      _._     (_)  `--'        \
 *          |   (   )    /   \                       \
 *           \   `-'     \   /                       ;-._
 *            \           `-'           \           .'   `.
 *            /`.                  `\    `\     _.-'`-.    `.___
 *           |   `-._                `\    `\.-'       `-.   ,--`
 *            \      `--.___        ___`\    \           ||^\\
 *             `._        | ``----''     `.   `\         `'  `
 *                `--;     \  jgs          `.   `.
 *                   //^||^\\               //^||^\\
 *                   '  `'  `               '   '  `
 ]]
