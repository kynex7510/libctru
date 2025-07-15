set(CTRU_SOURCE_PATHS
    source
	source/allocator
	source/gpu
	source/ndsp
	source/services
	source/services/soc
	source/applets
	source/util/decompress
	source/util/rbtree
	source/util/utf
	source/system
)

set(CTRU_SOURCES)

foreach(DIR IN LISTS CTRU_SOURCE_PATHS)
    foreach(EXT c cpp s)
        file(GLOB TMP_FILES CONFIGURE_DEPENDS "${DIR}/*.${EXT}")
        list(APPEND CTRU_SOURCES ${TMP_FILES})
    endforeach()
endforeach()

dkp_add_embedded_binary_library(default-font data/default_font.bin)

add_library(ctru STATIC ${CTRU_SOURCES})
target_include_directories(ctru PUBLIC include)
target_link_libraries(ctru PRIVATE default-font)

if (CMAKE_BUILD_TYPE STREQUAL "Release")
    target_compile_options(ctru PRIVATE "-fomit-frame-pointer")
endif()