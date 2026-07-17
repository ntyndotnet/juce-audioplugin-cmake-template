#[[  this is a comment  ]]

cmake_minimum_required(VERSION 3.26) # minimum version required for functionality

project(pluginName) # set PROJECT_NAME

set(CMAKE_CXX_STANDARD 23) # set C++ standard

set(CMAKE_MSVC_RUNTIME_LIBRARY "MultiThreaded$<$<CONFIG:Debug>:Debug>") # use static runtime

if (MSVC)
    # generate debugging info and .pdb files
    add_compile_options("$<$<NOT:$<CONFIG:Debug>>:/Zi>")
    add_link_options("$<$<NOT:$<CONFIG:Debug>>:/DEBUG>")

    # turn on some optimizations
    add_link_options("$<$<NOT:$<CONFIG:Debug>>:/OPT:REF>")
    add_link_options("$<$<NOT:$<CONFIG:Debug>>:/OPT:ICF>")
endif()

set(SOURCE_FILES
        source/PluginEditor.cpp
        source/PluginEditor.h
        source/PluginProcessor.cpp
        source/PluginProcessor.h) # put source files here

add_subdirectory(C:/libs/JUCE/juce-8.0.12-windows/JUCE JUCE) # put JUCE directory here

juce_add_plugin(${PROJECT_NAME}
    VERSION 0.0.0
    COMPANY_NAME ntyn
    IS_SYNTH FALSE
    NEEDS_MIDI_INPUT FALSE
    NEEDS_MIDI_OUTPUT FALSE
    IS_MIDI_EFFECT FALSE
    EDITOR_WANTS_KEYBOARD_FOCUS FALSE
    COPY_PLUGIN_AFTER_BUILD FALSE
    PLUGIN_MANUFACTURER_CODE ntyn
    PLUGIN_CODE plgn
    PRODUCT_NAME pluginName
    FORMATS VST3 Standalone)

juce_add_binary_data(Assets
    SOURCES
) # put assets to add as binary data here

target_sources(${PROJECT_NAME}  
    PRIVATE
       ${SOURCE_FILES}
)

target_include_directories(${PROJECT_NAME}
    PRIVATE
        source
)

target_compile_definitions(${PROJECT_NAME}
    PRIVATE
        JUCE_WEB_BROWSER=0
        JUCE_USE_CURL=0
        JUCE_VST3_CAN_REPLACE_VST2=0
)

target_link_libraries(${PROJECT_NAME}
    PRIVATE
        Assets

        juce::juce_audio_utils
        juce::juce_analytics
        juce::juce_audio_basics
        juce::juce_audio_devices
        juce::juce_core
        juce::juce_data_structures
        juce::juce_graphics
        juce::juce_gui_basics
        juce::juce_gui_extra
        juce::juce_dsp
        juce::juce_recommended_config_flags
        juce::juce_recommended_lto_flags
        juce::juce_recommended_warning_flags
)
