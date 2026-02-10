cmake_minimum_required(VERSION 3.10)

project(create_disp)


set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

add_executable(create-disp create-disp.cpp)

find_package(PkgConfig REQUIRED)
pkg_check_modules(ANDROID_HEADERS REQUIRED android-headers)
pkg_check_modules(LIBDRM REQUIRED libdrm)
pkg_check_modules(SYSTEMD REQUIRED libsystemd)

link_directories(${LIBDRM_LIBRARY_DIRS})

target_include_directories(create-disp PRIVATE
    ${ANDROID_HEADERS_INCLUDE_DIRS}
    ${LIBDRM_INCLUDE_DIRS}
    ${SYSTEMD_INCLUDE_DIRS}
)

target_link_libraries(create-disp
    hwc2
    hybris-common
    hybris-eglplatformcommon
    hybris-platformcommon
    gralloc
    ${LIBDRM_LIBRARIES}
    ${SYSTEMD_LIBRARIES}
)

configure_file(create-disp.service.template create-disp.service)
install(TARGETS create-disp DESTINATION bin)
install(FILES ${CMAKE_CURRENT_BINARY_DIR}/create-disp.service DESTINATION lib/systemd/system)
