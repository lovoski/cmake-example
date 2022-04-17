#specify the minmal version of the cmake version required
cmake_minimum_required (VERSION 3.10)
#specify the name of the project
project (demo)

#set the edition of the two languages
set(CMAKE_C_STANDARD 11)
set(CMAKE_CXX_STANDARD 11)

#set the toole chain so it can recognize library installed by vcpkg
set(CMAKE_TOOLCHAIN_FILE /home/lovoski/vcpkg/scripts/buildsystems/vcpkg.cmake)

#config the path of the binary file will be generated
#but be careful this is only the output diretory of the excutable file
set (EXECUTABLE_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/bin)

#add all the file in the src diretory
#mind that specify file can be declared as parameter using set()
aux_source_directory (src SRC_LIST)

#add all the file in the lib diretory
#I also looked into the function link_diretories(), but
#havn't figure out the use of it
aux_source_directory (lib LIB_LIST)

#make the include path can be found
#when using #include <>
#the file inside must be in this diretory, if extern libraries
#is used then the headers should be in the dir include as well
include_directories (include)

#generate shared library named hello-lib.so|hello-lib.dll
#this function add_libarary() will generate lib file directly
#use it carefully if you don't need to generate another lib file in the project
add_library(hello-lib-shared SHARED ${PROJECT_SOURCE_DIR}/lib/test_func.c)
#set some basic properties of the compiled lib file
set_target_properties(hello-lib-shared PROPERTIES OUTPUT_NAME "hello-lib-shared")
set(LIBRARY_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/bin)


#generate static library named hello-lib.lib
# !!!! somethong went wrong, it turnt out that no lib has the same name can co-exist!
#therefore the static lib version must be re-name
#I'm seriously wrong, one file can only be compiled to static or shared library in one time
#add_library(hello-lib-static STATIS ${PROJECT_SOURCE_DIR}/lib/test_func.c)
#set_target_properties(hello-lib-static PROPERTIES OUTPUT_NAME "hello-lib-static")
#set(LIBRARY_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/bin)

#the mst import job done
#generate the binary file
#associate the relatice files and generate the binary file
add_executable (main ${SRC_LIST} ${LIB_LIST})

#the function above should be used when the lib comes in source code
#however, when the lib is binary file *.a *.so for example, the following methods should be called
#add_executable(NAME_OF_EXECUTABLE ${SRC_LIST})
#target_link_libraries(NAME_OF_EXECUTABLE ${PROJECT_SOURCE_DIR}/lib/*.a)
#the parameter ${PROJECT_SOURCE_DIR}/lib/*.a can be specified to be any static lib file in the lib diretory