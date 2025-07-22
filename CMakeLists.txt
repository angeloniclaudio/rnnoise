cmake_minimum_required(VERSION 3.10)
project(rnnoise C)

# Cerca ricorsivamente tutti i .c, inclusi quelli in src/x86/
file(GLOB_RECURSE RNNOISE_SOURCES src/*.c)

# Crea libreria statica
add_library(rnnoise STATIC ${RNNOISE_SOURCES})

# Includi sia "include/" sia "src/"
target_include_directories(rnnoise
    PUBLIC
        ${CMAKE_CURRENT_SOURCE_DIR}/include
        ${CMAKE_CURRENT_SOURCE_DIR}/src
)

# Imposta standard C
set_target_properties(rnnoise PROPERTIES
    C_STANDARD 99
    C_STANDARD_REQUIRED ON
)

# Aggiungi flag AVX2 e SSE4.1 dove richiesto
foreach(source_file IN LISTS RNNOISE_SOURCES)
    if(source_file MATCHES "nnet_avx2.c")
        set_source_files_properties(${source_file} PROPERTIES COMPILE_FLAGS "-mavx2")
    elseif(source_file MATCHES "nnet_sse4_1.c")
        set_source_files_properties(${source_file} PROPERTIES COMPILE_FLAGS "-msse4.1")
    endif()
endforeach()

