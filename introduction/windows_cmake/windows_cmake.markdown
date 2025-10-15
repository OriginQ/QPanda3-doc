Compilation Environment Configuration for Windows {#tutorial_windows_cmake}
=================================================

@tableofcontents

@prev_tutorial{tutorial_system_requirement_linux}
@next_tutorial{tutorial_linux_cmake}

On Windows, you can use CMake to configure and generate compilation files.

1. Open the **Command Prompt** or **PowerShell**, then navigate to the root directory of QPanda3.

2. Create a build directory:

   ```bash
   mkdir build
   cd build
   ```

3. Use CMake to configure the project:

    ```bash
    cmake ..
    ```

4. Compile the project:

    ```bash
    cmake --build . --config Release
    ```

