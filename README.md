# -=- Here are the steps to install Zlib: -=-
- Delete the current /build folder.
- Open powershell in Visual Studio and cd to a folder, then type `git clone https://github.com/Microsoft/vcpkg.git`
- Type:
```
cd vcpkg
.\bootstrap-vcpkg.bat 
.\vcpkg integrate install
```
- The last command will give you an output like:
```
Applied user-wide integration for this vcpkg root.
CMake projects should use: "DCMAKE_TOOLCHAIN_FILE=<your_file_path>"

All MSBuild C++ projects can now #include any installed libraries. Linking will be handled automatically. Installing new libraries will make them instantly available.
```
- Copy the command it gives you, cd back to your project directory then run: `cmake -B build -S . -DCMAKE_TOOLCHAIN_FILE=<your_file_path>`, replacing with <your_file_path>.
- In CMakelists.txt paste this (copy your files includes first):
```
cmake_minimum_required(VERSION 3.10)
project(ECE141-Archive)

set(CMAKE_CXX_STANDARD 20)

find_package(ZLIB REQUIRED)

include_directories(.)

add_executable(archive
        <replace_with_your_file_includes>

if(ZLIB_FOUND)
    include_directories(${ZLIB_INCLUDE_DIRS})
    target_link_libraries(archive PRIVATE ${ZLIB_LIBRARIES})
endif()
```
- Finally, in Visual Studio right click CMakelists.txt, click "Edit CMake Presets for ..." OR click "CMake Settings" if you do not see the first one.
- In that file you have to add the "cmakeToolchain" to whichever preset you are using:
```
{
  "name": "x64-debug",
  "displayName": "x64 Debug",
  "description": "Target Windows (64-bit) with the Visual Studio development environment. (Debug)",
  "inherits": "windows-base",
  "architecture": {
    "value": "x64",
    "strategy": "external"
  },
  "cacheVariables": {
    "CMAKE_BUILD_TYPE": "Debug",
    "CMAKE_TOOLCHAIN_FILE": "<path_you_installed_vcpkg>/vcpkg/scripts/buildsystems/vcpkg.cmake"
  }
```
- #include "zlib.h" should work now!

