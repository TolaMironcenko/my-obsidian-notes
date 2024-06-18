
```txt
cmake_minimum_required (VERSION 2.6)  
project(project_name)  
  
include_directories(./src/libs/)  
  
find_package(package-name REQUIRED)  
  
add_executable(  
    project_name   
    src/project_name.c
)  
target_link_libraries(project_name package-name::package-name)
```