
```txt
cmake_minimum_required (VERSION 2.6)  
project(project_name)  
set(CMAKE_CXX_STANDARD 17)  
  
include_directories(./src/libs/)  
  
find_package(package-name REQUIRED)  
  
add_executable(  
    project_name   
    src/project_name.cpp 
)  
target_link_libraries(project_name package-name::package-name)
```
