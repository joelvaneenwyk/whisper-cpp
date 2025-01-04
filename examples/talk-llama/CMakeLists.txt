if (WHISPER_SDL2)
    set(TARGET whisper-talk-llama)
    add_executable(${TARGET} talk-llama.cpp
        llama.cpp
        llama-vocab.cpp
        llama-grammar.cpp
        llama-sampling.cpp
        unicode.cpp
        unicode-data.cpp)
    target_include_directories(${TARGET} PRIVATE ${SDL2_INCLUDE_DIRS})

    if (WHISPER_CLBLAST)
        set(CLBLAST_LIBNAME clblast)
    endif ()

    target_link_libraries(${TARGET}
        PRIVATE
        common common-sdl whisper
        ${CLBLAST_LIBNAME} ${CMAKE_THREAD_LIBS_INIT}
        $<TARGET_NAME_IF_EXISTS:SDL2::SDL2main>
        $<IF:$<TARGET_EXISTS:SDL2::SDL2>,SDL2::SDL2,SDL2::SDL2-static>
    )

    if(WIN32)
        # It requires Windows 8.1 or later for PrefetchVirtualMemory
        target_compile_definitions(${TARGET} PRIVATE -D_WIN32_WINNT=0x0602)
    endif()

    include(DefaultTargetOptions)
endif ()
