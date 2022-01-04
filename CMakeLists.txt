cmake_minimum_required(VERSION 2.8.11)

SET(PROJECT_NAME C2A)
project(${PROJECT_NAME})

set(VERSION_MAJOR 1)
set(VERSION ${VERSION_MAJOR})

set( HDRS
	${PROJECT_NAME}/include/C2A.h
	${PROJECT_NAME}/include/C2A_BV.h
	${PROJECT_NAME}/include/C2A_Internal.h
	${PROJECT_NAME}/include/C2A_RectDist.h
	${PROJECT_NAME}/include/C2A_Tri.h
	${PROJECT_NAME}/include/InterpMotion.h
	${PROJECT_NAME}/include/LinearMath.h
	${PROJECT_NAME}/include/ListTri.h
)

set( SRCS
	${PROJECT_NAME}/src/C2A.cpp
	${PROJECT_NAME}/src/C2A_Build.cpp
	${PROJECT_NAME}/src/C2A_BV.cpp
	${PROJECT_NAME}/src/C2A_PQP.cpp
	${PROJECT_NAME}/src/InterpMotion.cpp
)

set( DEMO_SRCS
	demo/src/main.cpp
	demo/src/model.cpp
	demo/src/stopwatch.cpp
)

add_subdirectory(PQP)
add_library(${PROJECT_NAME} STATIC ${HDRS} ${SRCS})
add_dependencies(${PROJECT_NAME} PQP)

target_include_directories(${PROJECT_NAME} PUBLIC ${PROJECT_NAME}/include)
target_link_libraries(${PROJECT_NAME} PQP)


option(BUILD_SAMPLES "Build samples" ON)

if(BUILD_SAMPLES)
	find_package(OpenGL)
	find_package(GLUT)

	add_executable(demo ${DEMO_SRCS})
	target_include_directories(demo PUBLIC demo/include ${OPENGL_INCLUDE_DIRS} ${GLUT_INCLUDE_DIR})
	target_link_libraries(demo C2A PQP ${OPENGL_LIBRARIES} ${GLUT_LIBRARIES})
endif()

