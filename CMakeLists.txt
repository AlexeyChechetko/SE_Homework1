cmake_minimum_required(VERSION 3.14)
project(Homework1)

include_directories(include)

add_executable(main main.cpp src/my_add.cpp)

include(FetchContent)
FetchContent_Declare(
	googletest
	GIT_REPOSITORY https://github.com/google/googletest.git
	GIT_TAG v1.15.2
)

if (WIN32)
	set(gtest_force_shared_crt ON CACHE BOOL "" FORCE)
endif()

FetchContent_MakeAvailable(googletest)

enable_testing()
add_executable(tests test_add.cpp src/my_add.cpp)
target_link_libraries(tests gtest_main)

FetchContent_Declare(
	benchmark
	GIT_REPOSITORY https://github.com/google/benchmark.git
	GIT_TAG v1.9.0
)
FetchContent_MakeAvailable(benchmark)

add_executable(benchmarks benchmark_test.cpp src/my_add.cpp)
target_link_libraries(benchmarks benchmark::benchmark)

include(GoogleTest)
gtest_discover_tests(tests)


find_package(Doxygen REQUIRED)

if(DOXYGEN_FOUND)

	# Укажите параметры для Doxygen 
	set(DOXYGEN_IN ${CMAKE_CURRENT_SOURCE_DIR}/Doxyfile.in) 
	set(DOXYGEN_OUT ${CMAKE_CURRENT_BINARY_DIR}/Doxyfile) 
 
	# Создайте Doxyfile из шаблона 
	configure_file(${DOXYGEN_IN} ${DOXYGEN_OUT} @ONLY) 
 
	# Добавьте команду для генерации документации 
	add_custom_target(doc ALL 
    		COMMAND ${DOXYGEN_EXECUTABLE} ${DOXYGEN_OUT} 
    		WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR} 
		COMMENT "Generation documentation with Doxygen" 
	)	
endif()



