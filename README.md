# MyProject
Author: Leonardo Celente

# What is it?


# How do I build it?
## Dependencies
 - CMake 
 - C++20
 - Conan Package Manager
    - fmt
    - catch2
    - ctre
    - cxxopts
    - range-v3

## Steps
```bash
mkdir build
cd build
cmake .. 
cmake --build . --config Debug
```

Executables should be in `build/bin`
