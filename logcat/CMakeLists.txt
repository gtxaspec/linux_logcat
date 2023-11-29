PROJECT(logcat)

CMAKE_MINIMUM_REQUIRED(VERSION 3.0)

# Compiler definitions
add_definitions("-DHAVE_SYS_UIO_H -DHAVE_IOCTL")
add_definitions("-DLOGGER_LOG_MAIN=\"log_main\"")
add_definitions("-DLOGGER_LOG_RADIO=\"log_radio\"")
add_definitions("-DLOGGER_LOG_EVENTS=\"log_events\"")
add_definitions("-DLOGGER_LOG_SYSTEM=\"log_system\"")

# Compiler and linker flags
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -Os -ffunction-sections -fdata-sections -fomit-frame-pointer -flto")
set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} -static-libstdc++ -Wl,--gc-sections -flto")

AUX_SOURCE_DIRECTORY(. SRC_LIST)
INCLUDE_DIRECTORIES(${PROJECT_SOURCE_DIR}/../include)
ADD_EXECUTABLE(logcat ${SRC_LIST})

# Strip the binary
add_custom_command(TARGET logcat POST_BUILD
                   COMMAND ${CMAKE_STRIP} $<TARGET_FILE:logcat>)

install(TARGETS logcat
        DESTINATION ${CMAKE_INSTALL_PREFIX}/bin)
