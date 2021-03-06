if (EKA2L1_ENABLE_SCRIPTING_ABILITY)
    set(SCRIPT_MANAGER_FILES
            include/manager/script_manager.h
            src/script_manager.cpp)
endif ()

add_library(manager
        include/manager/installation/raw_dump.h
        include/manager/installation/rpkg.h
        include/manager/app_settings_manager.h
        include/manager/device_manager.h
        include/manager/manager.h
        include/manager/package_manager.h
        include/manager/sis_common.h
        include/manager/sis_fields.h
        include/manager/sis_old.h
        include/manager/sis_script_interpreter.h
        include/manager/sis_v1_installer.h
        include/manager/sis.h
        include/manager/software.h
        src/installation/raw_dump.cpp
        src/installation/rpkg.cpp
        src/app_settings_manager.cpp
        src/device_manager.cpp
        src/manager.cpp
        src/package_manager.cpp
        src/sis_fields.cpp
        src/sis_old.cpp
        src/sis_script_interpreter.cpp
        src/sis_v1_installer.cpp
        src/sis.cpp
        src/software.cpp
        ${SCRIPT_MANAGER_FILES})

target_include_directories(manager PUBLIC include)

# Python libraries should be shared since i don't want passing library around and
# it will be used very common
target_link_libraries(manager PUBLIC common)

if (EKA2L1_ENABLE_SCRIPTING_ABILITY)
    target_link_libraries(manager PUBLIC pybind11::embed)
    target_link_libraries(manager PRIVATE symemu epoc epockern)

    if (NOT MSVC)
        target_link_libraries(manager PRIVATE stdc++fs)
    endif ()
endif (EKA2L1_ENABLE_SCRIPTING_ABILITY)

target_link_libraries(manager PRIVATE config epoc epockern epocmem epocservs miniz yaml-cpp)
