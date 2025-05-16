cmake_minimum_required(VERSION 3.21)
project(AltSnap VERSION 1.6.4)

set(CMAKE_EXPORT_COMPILE_COMMANDS 1)

set(LDFLAGS -lmsvcrt -lkernel32 -luser32 -lgdi32 -lwinmm)
set(EXELD ${LDFLAGS} -lcomctl32 -ladvapi32 -lshell32)

set(CMAKE_EXE_LINKER_FLAGS )

add_library(hooks SHARED src/hooks.c)
target_sources(hooks PRIVATE res/hooks.rc)
target_compile_options(hooks PRIVATE -mdll)
target_link_libraries(hooks PRIVATE ${LDFLAGS})
target_include_directories(hooks PRIVATE include res)

# target_compile_definitions(hooks PUBLIC LOG_STUFF)

add_executable(AltSnap WIN32 src/altsnap.c)
target_sources(AltSnap PRIVATE res/altsnap.rc)
target_link_libraries(AltSnap PRIVATE ${EXELD} hooks)
target_compile_options(AltSnap PRIVATE )
target_include_directories(AltSnap PRIVATE include)
target_link_options(AltSnap PRIVATE -nostdlib -e unfuckWinMain)

file(CREATE_LINK ${CMAKE_SOURCE_DIR}/Lang
     ${CMAKE_CURRENT_BINARY_DIR}/Lang
     SYMBOLIC)

file(COPY_FILE ${CMAKE_SOURCE_DIR}/AltSnap.dni ${CMAKE_CURRENT_BINARY_DIR}/AltSnap.dni ONLY_IF_DIFFERENT)
file(COPY_FILE ${CMAKE_SOURCE_DIR}/AltSnap.txt ${CMAKE_CURRENT_BINARY_DIR}/AltSnap.txt ONLY_IF_DIFFERENT)
file(COPY_FILE ${CMAKE_SOURCE_DIR}/AltSnap.xml ${CMAKE_CURRENT_BINARY_DIR}/AltSnap.xml ONLY_IF_DIFFERENT)
file(COPY_FILE ${CMAKE_SOURCE_DIR}/License.txt ${CMAKE_CURRENT_BINARY_DIR}/License.txt ONLY_IF_DIFFERENT)

install(TARGETS AltSnap hooks DESTINATION .)
install(FILES AltSnap.txt AltSnap.xml AltSnap.dni License.txt DESTINATION .)
install(DIRECTORY Lang DESTINATION .)

set(CPACK_GENERATOR "ZIP")

include(CPack)