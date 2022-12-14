project(systeminfo)

find_package(Qt5Core)

set(systeminfo_SOURCES
    include/sys.h
    include/distroutils.h
    src/distroutils.cpp
)

set(systeminfo_INCLUDE_DIRS
        ${CMAKE_CURRENT_LIST_DIR}/include)

if (WIN32)
    list(APPEND systeminfo_SOURCES src/sys_win32.cpp src/ntstatus/NtStatusNames.cpp)
elseif (UNIX)
    if(APPLE)
        list(APPEND systeminfo_SOURCES src/sys_apple.cpp)
    else()
        list(APPEND systeminfo_SOURCES src/sys_unix.cpp)
    endif()
endif()

add_library(systeminfo STATIC ${systeminfo_SOURCES})
target_link_libraries(systeminfo Qt5::Core Qt5::Gui Qt5::Network)
target_include_directories(systeminfo PUBLIC ${systeminfo_INCLUDE_DIRS})

include (UnitTest)
add_unit_test(sys
    SOURCES src/sys_test.cpp
    LIBS systeminfo
)
