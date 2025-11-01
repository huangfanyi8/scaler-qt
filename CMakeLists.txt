        cmake_minimum_required(VERSION 3.20)
        project(scaler-qt)

        set(CMAKE_CXX_STANDARD 23)
        set(CMAKE_PREFIX_PATH  "C:/Qt/6.9.3/mingw_64")
        message("source dir is ${CMAKE_SOURCE_DIR}")
        set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_SOURCE_DIR}/bin)
        set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_SOURCE_DIR}/lib)
        set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_SOURCE_DIR}/lib)
        set(CMAKE_AUTOMOC  ON)
        set(CMAKE_AUTORCC ON)
        set(CMAKE_AUTOUIC ON)

        find_package(Qt6 COMPONENTS Core Gui Widgets Svg REQUIRED)

        function( scaler_add_sources   return source_dir)
            file(GLOB_RECURSE _return
                    scaler-qt/${source_dir}/*.*
                    test/${source_dir}/*.cpp)
            set(${return} ${_return} PARENT_SCOPE)
        endfunction()

        function(scaler_add_executable src_dir exe_name)
            scaler_add_sources(sources ${src_dir})
            set(exe_combine_name "${exe_name}_${src_dir}")
            add_executable(${exe_combine_name} ${sources} )
            foreach (src ${sources})
                message("add source ${src}")
            endforeach ()
            set(modules   ${ARGN})
            foreach (qt6_module IN LISTS modules)
                message("Current linked directory is ${qt6_module}")
                target_link_libraries(${exe_combine_name}  PUBLIC ${qt6_module})
            endforeach ()
            message("The number  of Arguments is  ${ARGC}")
        endfunction()

        scaler_add_executable(main main  Qt6::Widgets )






