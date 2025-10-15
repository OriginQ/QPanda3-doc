System requirements for Linux {#tutorial_system_requirement_linux}
=============================

@tableofcontents

@prev_tutorial{tutorial_system_requirement_windows}
@next_tutorial{tutorial_windows_cmake}

Lists the system requirements for compiling and running the library for Linux.

- Operating System: Ubuntu 20.04 or higher (other Linux distributions also apply, different package managers may be required)
- Compiler: GCC 9.0 or higher, or Clang
- CMake 3.18 or higher
- Git (for cloning library code)
- Necessary dependency libraries (such as Boost graph, mypy, setuptools.)

1. **Install CMake**:
   In the terminal, execute the following command to install CMake:
   
   ```bash
   sudo apt update
   sudo apt install cmake
   ```
2. **Install GCC or Clang**: Install GCC (assuming Ubuntu is used):

    ```bash
    sudo apt install build-essential
    ```

3. **Install Git**
   
    ```bash
    sudo apt install git
    ```

