cmake_minimum_required(VERSION 3.14)
include(FetchContent)

project(kortex_api)
find_package(ament_cmake REQUIRED)

if(CMAKE_SYSTEM_NAME STREQUAL "Linux" AND CMAKE_SYSTEM_PROCESSOR STREQUAL "x86_64")
  set(API_URL https://artifactory.kinovaapps.com:443/artifactory/generic-public/kortex/API/2.5.0/linux_x86-64_x86_gcc.zip)
	set(URL_HASH_MD5 64bd86e7ab8bda90ef1fc7d6a356e080)
elseif(CMAKE_SYSTEM_NAME STREQUAL "Linux" AND CMAKE_SYSTEM_PROCESSOR STREQUAL "aarch64")
  set(API_URL https://artifactory.kinovaapps.com:443/artifactory/generic-public/kortex/API/2.6.0/linux_aarch64_gcc_7.4.zip)
	set(URL_HASH_MD5 d7771a7c106888717ace359164d40235)
else()
  set(API_URL "")
  message(FATAL_ERROR "Unsupported System: currently support is for Linux on x86_64 or aarch4. Detected ${CMAKE_SYSTEM_NAME} and ${CMAKE_SYSTEM_PROCESSOR}")
endif()

FetchContent_Declare(
  kinova_binary_api
  DOWNLOAD_EXTRACT_TIMESTAMP true
  URL ${API_URL}
  URL_HASH MD5=${URL_HASH_MD5}
)

FetchContent_MakeAvailable(kinova_binary_api)

ament_export_include_directories(include/kortex_api)

install(DIRECTORY ${kinova_binary_api_SOURCE_DIR}/include/client/ DESTINATION include/kortex_api)
install(DIRECTORY ${kinova_binary_api_SOURCE_DIR}/include/common/ DESTINATION include/kortex_api)
install(DIRECTORY ${kinova_binary_api_SOURCE_DIR}/include/messages/ DESTINATION include/kortex_api)
install(DIRECTORY ${kinova_binary_api_SOURCE_DIR}/include/client_stubs/ DESTINATION include/kortex_api)
install(DIRECTORY ${kinova_binary_api_SOURCE_DIR}/include/google/protobuf DESTINATION include/kortex_api/google)

ament_package()
