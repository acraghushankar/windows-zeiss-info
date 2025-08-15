# windows-zeiss-info
https://learn.microsoft.com/en-us/vcpkg/get_started/get-started-vs?pivots=shell-powershell
git clone https://github.com/microsoft/vcpkg.git
cd vcpkg; .\bootstrap-vcpkg.bat
$env:VCPKG_ROOT="C:\path\to\vcpkg"
$env:PATH="$env:VCPKG_ROOT;$env:PATH"
Add VCPKG_ROOT in system and user environmental tab of windows
vcpkg new --application
vcpkg add port fmt
#include <fmt/core.h>

int main()
{
    fmt::print("Hello World!\n");
    return 0;
}

Configure the CMakePresets.json file.

CMake can automatically link libraries installed by vcpkg when CMAKE_TOOLCHAIN_FILE is set to use vcpkg's custom toolchain. This can be acomplished using CMake presets files.

Modify CMakePresets.json to match the content below:

JSON

Copy
{
  "version": 2,
  "configurePresets": [
    {
      "name": "vcpkg",
      "generator": "Ninja",
      "binaryDir": "${sourceDir}/build",
      "cacheVariables": {
        "CMAKE_TOOLCHAIN_FILE": "$env{VCPKG_ROOT}/scripts/buildsystems/vcpkg.cmake"
      }
    }
  ]
}
Create CMakeUserPresets.json with the following content:


Edit the CMakeLists.txt file.

Replace the contents of the CMakeLists.txt file with the following code:

cmake

Copy
cmake_minimum_required(VERSION 3.10)

project(HelloWorld)

find_package(fmt CONFIG REQUIRED)

add_executable(HelloWorld helloworld.cpp)

target_link_libraries(HelloWorld PRIVATE fmt::fmt)
