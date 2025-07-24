cmake_minimum_required(VERSION 3.20)
project(finance-app)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

find_package(Boost REQUIRED COMPONENTS program_options json)
find_package(PkgConfig REQUIRED)
pkg_check_modules(PQXX REQUIRED libpqxx)
find_package(Eigen3 REQUIRED)
find_package(CURL REQUIRED)

add_executable(finance-app
    src/main.cpp
    src/services/ControlService.cpp
    src/services/DataFetcherService.cpp
    src/services/AnalyticsService.cpp
    src/services/JsonExporterService.cpp
    src/repository/PostgreSQLRepository.cpp
)

target_include_directories(finance-app PRIVATE src/)
target_link_libraries(finance-app 
    Boost::program_options 
    Boost::json
    ${PQXX_LIBRARIES}
    Eigen3::Eigen
    CURL::libcurl
)

target_include_directories(finance-app PRIVATE ${PQXX_INCLUDE_DIRS})
target_compile_options(finance-app PRIVATE ${PQXX_CFLAGS_OTHER})
