cmake_minimum_required(VERSION 3.20)
project(AltSnap VERSION 1.0)

set(CMAKE_EXPORT_COMPILE_COMMANDS 1)

set(LDFLAGS -lmsvcrt -lkernel32 -luser32 -lgdi32 -lwinmm)
set(EXELD ${LDFLAGS} -lcomctl32 -ladvapi32 -lshell32)

set(CMAKE_EXE_LINKER_FLAGS )

add_library(hooks SHARED src/hooks.c)
target_sources(hooks PRIVATE res/hooks.rc)
target_compile_options(hooks PRIVATE -mdll)
target_link_libraries(hooks PRIVATE ${LDFLAGS})
target_include_directories(hooks PRIVATE include res)


add_executable(AltSnap WIN32 src/altsnap.c)
target_sources(AltSnap PRIVATE res/altsnap.rc)
target_link_libraries(AltSnap PRIVATE ${EXELD} hooks)
target_compile_options(AltSnap PRIVATE )
target_include_directories(AltSnap PRIVATE include)
target_link_options(AltSnap PRIVATE -nostdlib -e unfuckWinMain)

