cmake_minimum_required(VERSION 3.16)
project(mpg123 CXX)


message(STATUS "libmpg123 Library")

list(APPEND library_source
   src/libmpg123/abi_align.h
   src/libmpg123/compat.c
   src/libmpg123/dct64.c
   src/libmpg123/debug.h
   src/libmpg123/decode.h
   src/libmpg123/dither.c
   src/libmpg123/dither.h
   src/libmpg123/dither_impl.h
   src/libmpg123/equalizer.c
   src/libmpg123/feature.c
   src/libmpg123/fmt123.h
   src/libmpg123/format.c
   src/libmpg123/frame.c
   src/libmpg123/frame.h
   src/libmpg123/gapless.h
   src/libmpg123/getbits.h
   src/libmpg123/getcpuflags.h
   src/libmpg123/huffman.h
   src/libmpg123/icy.c
   src/libmpg123/icy.h
   src/libmpg123/icy2utf8.c
   src/libmpg123/icy2utf8.h
   src/libmpg123/id3.c
   src/libmpg123/id3.h
   src/libmpg123/index.c
   src/libmpg123/index.h
   src/libmpg123/l2tables.h
   src/libmpg123/l3_integer_tables.h
   src/libmpg123/l12_integer_tables.h
   src/libmpg123/layer1.c
   src/libmpg123/layer2.c
   src/libmpg123/layer3.c
   src/libmpg123/libmpg123.c
   src/libmpg123/mangle.h
   src/libmpg123/mpeghead.h
   src/libmpg123/mpg123lib_intern.h
   src/libmpg123/newhuffman.h
   src/libmpg123/ntom.c
   src/libmpg123/optimize.c
   src/libmpg123/optimize.h
   src/libmpg123/parse.c
   src/libmpg123/parse.h
   src/libmpg123/reader.h
   src/libmpg123/readers.c
   src/libmpg123/sample.h
   src/libmpg123/stringbuf.c
   src/libmpg123/synth.c
   src/libmpg123/synth.h
   src/libmpg123/synth_8bit.c
   src/libmpg123/synth_8bit.h
   src/libmpg123/synth_mono.h
   src/libmpg123/synth_ntom.h
   src/libmpg123/synth_real.c
   src/libmpg123/synth_s32.c
   src/libmpg123/synths.h
   src/libmpg123/tabinit.c
   #src/libmpg123/testcpu.c
   src/libmpg123/true.h
   )

if(${ALTIVEC_ARCH})

    list(APPEND library_source
       src/libmpg123/dct64_altivec.c
       src/libmpg123/synth_altivec.c

    )

endif()


if(${LFS_ALIAS})

list(APPEND library_source
src/libmpg123/lfs_alias.c
)

endif()


if(${LFS_WRAP})

list(APPEND library_source
   src/libmpg123/lfs_wrap.c
)

endif()


if(${ANDROID})

if (ANDROID_ABI STREQUAL "arm64-v8a")
    message("Building MPG123 for ARM64 (arm64-v8a)")
    list(APPEND library_source
       src/libmpg123/getcpuflags_arm.c

       src/libmpg123/check_neon.S

       src/libmpg123/dct36_neon64.S

       src/libmpg123/dct64_neon64.S
       src/libmpg123/dct64_neon64_float.S

       src/libmpg123/synth_neon64.S
       src/libmpg123/synth_neon64_accurate.S
       src/libmpg123/synth_neon64_float.S
       src/libmpg123/synth_neon64_s32.S

       src/libmpg123/synth_stereo_neon64.S
       src/libmpg123/synth_stereo_neon64_accurate.S
       src/libmpg123/synth_stereo_neon64_float.S
       src/libmpg123/synth_stereo_neon64_s32.S
    )
elseif (ANDROID_ABI STREQUAL "armeabi-v7a")
    message("Building MPG123 for ARMv7-A (armeabi-v7a)")
    list(APPEND library_source

       src/libmpg123/getcpuflags_arm.c

       src/libmpg123/check_neon.S

       src/libmpg123/dct36_neon.S

       src/libmpg123/dct64_neon.S
       src/libmpg123/dct64_neon_float.S

       src/libmpg123/synth_neon.S
       src/libmpg123/synth_neon_accurate.S
       src/libmpg123/synth_neon_float.S
       src/libmpg123/synth_neon_s32.S
       src/libmpg123/synth_arm.S
       src/libmpg123/synth_arm_accurate.S

       src/libmpg123/synth_stereo_neon.S
       src/libmpg123/synth_stereo_neon_accurate.S
       src/libmpg123/synth_stereo_neon_float.S
       src/libmpg123/synth_stereo_neon_s32.S

    )
else()
    # Code for other Android ABIs or non-Android builds
    message("Building for a different ABI or non-Android platform: ${ANDROID_ABI}")
endif()

endif()


if(WIN32)

    #list(APPEND audio_source
       #id3lib/windows/tts_speaker.cpp
       #id3lib/windows/tts_speaker.h
     #  )

endif()

if (ANDROID_ABI STREQUAL "x86")
    message("Building MPG123 for x86)")
    list(APPEND library_source

       src/libmpg123/synth_i486.c

       src/libmpg123/dct64_i386.c
       src/libmpg123/dct64_i486.c

       src/libmpg123/getcpuflags.S
       src/libmpg123/dct36_3dnow.S
       src/libmpg123/dct36_3dnowext.S
       src/libmpg123/dct36_sse.S
       src/libmpg123/dct64_3dnow.S
       src/libmpg123/dct64_3dnowext.S

       src/libmpg123/dct64_mmx.S
       src/libmpg123/dct64_sse.S
       src/libmpg123/dct64_sse_float.S
       src/libmpg123/equalizer_3dnow.S


       src/libmpg123/synth_i586.S
       src/libmpg123/synth_i586_dither.S
       src/libmpg123/synth_mmx.S
       src/libmpg123/tabinit_mmx.S

       src/libmpg123/synth_stereo_sse_accurate.S
       src/libmpg123/synth_stereo_sse_float.S
       src/libmpg123/synth_stereo_sse_s32.S
       src/libmpg123/synth_sse.S
       src/libmpg123/synth_sse3d.h
       src/libmpg123/synth_sse_accurate.S
       src/libmpg123/synth_sse_float.S
       src/libmpg123/synth_sse_s32.S
       src/libmpg123/synth_3dnow.S
       src/libmpg123/synth_3dnowext.S

    )
elseif (ANDROID_ABI STREQUAL "x86_64")
        message("Building MPG123 for Android ARMv7-A (armeabi-v7a)")
        list(APPEND library_source

           src/libmpg123/getcpuflags_x86_64.S
           src/libmpg123/dct36_avx.S
           src/libmpg123/dct36_x86_64.S

           src/libmpg123/dct64_avx.S
           src/libmpg123/dct64_avx_float.S

           src/libmpg123/dct64_x86_64.S
           src/libmpg123/dct64_x86_64_float.S

           src/libmpg123/synth_stereo_x86_64.S
           src/libmpg123/synth_stereo_x86_64_accurate.S
           src/libmpg123/synth_stereo_x86_64_float.S
           src/libmpg123/synth_stereo_x86_64_s32.S
           src/libmpg123/synth_x86_64.S
           src/libmpg123/synth_x86_64_accurate.S
           src/libmpg123/synth_x86_64_float.S
           src/libmpg123/synth_x86_64_s32.S
           src/libmpg123/synth_stereo_avx.S
           src/libmpg123/synth_stereo_avx_accurate.S
           src/libmpg123/synth_stereo_avx_float.S
           src/libmpg123/synth_stereo_avx_s32.S

        )
endif()

add_library(${PROJECT_NAME} SHARED ${library_source})
add_library(static_${PROJECT_NAME} STATIC ${library_source})


#target_precompile_headers(${PROJECT_NAME} PRIVATE framework.h)
#target_precompile_headers(static_${PROJECT_NAME} PRIVATE framework.h)


set_target_properties(${PROJECT_NAME} PROPERTIES LINKER_LANGUAGE CXX)

target_compile_definitions(${PROJECT_NAME} PRIVATE HAVE_CONFIG_H OPT_GENERIC_DITHER)
target_compile_definitions(static_${PROJECT_NAME} PRIVATE HAVE_CONFIG_H OPT_GENERIC_DITHER)


if (MSVC)
    target_compile_options(${PROJECT_NAME} PRIVATE "/MD$<$<CONFIG:Debug>:d>")
    target_link_options(${PROJECT_NAME} PRIVATE "/DEFAULTLIB:msvcrt$<$<CONFIG:Debug>:d>")
    target_link_options(${PROJECT_NAME} PRIVATE "/SUBSYSTEM:WINDOWS")
    target_link_libraries(${PROJECT_NAME} PRIVATE ${library_references} ${global_library_references})
    target_link_libraries(${PROJECT_NAME} PRIVATE ws2_32 winmm shlwapi zlib)
elseif (${LINUX} OR ${__BSD__} OR ${ANDROID})


    target_compile_options(${PROJECT_NAME} PRIVATE $<$<COMPILE_LANGUAGE:CXX>:-fpermissive  ${EXTRA_CXX_TARGET_COMPILER_OPTIONS}>)
    target_compile_options(static_${PROJECT_NAME} PRIVATE $<$<COMPILE_LANGUAGE:CXX>:-fpermissive  ${EXTRA_CXX_TARGET_COMPILER_OPTIONS}>)
    target_compile_definitions(static_${PROJECT_NAME} PRIVATE CUBE)


    
    target_link_libraries(${PROJECT_NAME} PRIVATE z)
    target_link_libraries(${PROJECT_NAME} PRIVATE ${library_references} ${global_library_references})
    target_link_options(${PROJECT_NAME} PRIVATE -Wl,-z,defs)
    # target_link_options(static_${PROJECT_NAME} PRIVATE --whole-archive)


endif()




list(APPEND library_include_directories ${CMAKE_CURRENT_SOURCE_DIR}/../..)
list(APPEND library_include_directories ${CMAKE_CURRENT_SOURCE_DIR}/..)
list(APPEND library_include_directories ${CMAKE_CURRENT_SOURCE_DIR})
list(APPEND library_include_directories ${CMAKE_CURRENT_SOURCE_DIR}/include)
list(APPEND library_include_directories ${CMAKE_CURRENT_SOURCE_DIR}/src)
list(APPEND library_include_directories ${CMAKE_CURRENT_SOURCE_DIR}/src/compat)
list(APPEND library_include_directories ${CMAKE_CURRENT_SOURCE_DIR}/src/libmpg123)
list(APPEND library_include_directories ${CMAKE_CURRENT_SOURCE_DIR}/../../../operating_system/operating_system-${PLATFORM_NAME}/configuration/${PROJECT_NAME})



target_include_directories(${PROJECT_NAME} PRIVATE ${library_include_directories})
target_include_directories(static_${PROJECT_NAME} PRIVATE ${library_include_directories})


target_include_directories(${PROJECT_NAME} PRIVATE ${CMAKE_CURRENT_SOURCE_DIR}/src)


target_compile_definitions(${PROJECT_NAME} PRIVATE _${PROJECT_NAME}_project)



