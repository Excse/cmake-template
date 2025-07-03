# cmake-template

A template structure for a modern CMake project, offering a robust starting point for C++ library development with CMake 3.20+.

---

## Features

- **Modern CMake** (3.20+)
- **C++20** enabled by default
- Out-of-the-box **library target** with namespaced alias
- **Header and source separation**: designed for libraries
- **Exported CMake config** for easy consumption via `find_package`
- **Testing setup** using GoogleTest (via FetchContent)
- Clean, minimal, and ready to extend for your project

---

## Getting Started

### 1. Clone the template

```sh
git clone https://github.com/Excse/cmake-template.git
cd cmake-template
````

### 2. Configure the project
There are several options you can enable/disable to your liking (or even remove the code associated with the option):
 - ``BUILD_TESTING``: Also builds the test executable, which will make the build process way longer. (Disable it if you just want to create an executable or installable library)
 - ``BUILD_EXECUTABLE``: You have to specify a ``main.cpp`` in ``/src`` to build an executable that can use your library.
 - ``MAKE_INSTALLABLE``: Depending on the option the library can be installed or just build.
 - ``BUILD_SHARED_LIBS``: An option to declare if the libray should be shared or static.


```sh
cmake -S . -B build # -DBUILD_TESTING=OFF -DBUILD_EXECUTABLE=OFF -DMAKE_INSTALLABLE=OFF -DBUILD_SHARED_LIBS=OFF
````

### 3. Build the library (and executable, if enabled)

```sh
cmake --build build
````

### 4. Run Unit-Tests (only if enabled)

```sh
cd build
ctest
````

### 5. Install the library (only if enabled)

```sh
cd build
sudo make install
````

### 6. Uninstall the library (only if the library was installed)

```sh
cd build
cat install_manifest.txt | sudo xargs rm
```

---

## Usage
After building and installing, you can use this library in another CMake project via:

```cmake
find_package(your_project REQUIRED)
target_link_libraries(your_target PRIVATE your_project::your_project)
```

---

## Project Structure

```code
cmake-template/
├── CMakeLists.txt
├── include/
│   └── your_project/
│       └── library.hpp  # (sample header, add your interfaces here)
├── src/
│   ├── main.cpp         # (main file, for the executable)
│   └── library.cpp      # (sample implementation)
├── tests/
│   └── CMakeLists.txt   # (GoogleTest auto-download and test integration)
```

---

## Testing
 - GoogleTest is automatically downloaded using CMake's FetchContent.
 - All .cpp files in tests/ are compiled and run as unit tests.

--- 

## Customization
 - Change project(your_project ...) in CMakeLists.txt to your project name.
 - Add your source/header files under src/ and include/.
 - Adjust install rules as needed for your distribution.
 - Enable/Disable options or remove the dependent code

---

## License
This project is a template and does not include a license by default. Add a LICENSE file as appropriate for your use.