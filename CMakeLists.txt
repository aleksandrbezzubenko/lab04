cmake_minimum_required(VERSION 3.4)
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

project(formatter_inc)

add_library(formatter STATIC formatter_lib/formatter.cpp formatter_lib/formatter.h)

add_library(formatter_ex STATIC formatter_ex_lib/formatter_ex.cpp formatter_ex_lib/formatter_ex.h)
target_link_libraries(formatter_ex formatter)

add_library(solver_lib STATIC solver_lib/solver.cpp solver_lib/solver.h)

add_executable(solver solver_application/equation.cpp)
target_link_libraries(solver formatter_ex)
target_link_libraries(solver solver_lib)

add_executable(hello_world hello_world_application/hello_world.cpp)
target_link_libraries(hello_world formatter_ex)

include_directories(session PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}/formatter_lib)
include_directories(session PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}/formatter_ex_lib)
include_directories(session PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}/solver_lib)

install(TARGETS solver
    EXPORT solver-config
    ARCHIVE DESTINATION lib
    LIBRARY DESTINATION lib
    RUNTIME DESTINATION lib
)

install(DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/formatter_lib/ DESTINATION formatter_lib)
install(DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/formatter_ex_lib/ DESTINATION formatter_ex_lib)
install(DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/solver_lib/ DESTINATION solver_lib)
install(EXPORT solver-config DESTINATION cmake)
