# Sourcefiles for primary executable
file(GLOB SOURCE_FILES *.cpp)

add_executable(test-executable ${SOURCE_FILES})

target_link_libraries(test-executable
        libgtest
        libgmock
        libgmock_main)

add_test(NAME test-executable
        COMMAND test-executable)
