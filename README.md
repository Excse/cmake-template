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

### 1. Clone the Template

```sh
git clone https://github.com/Excse/cmake-template.git
cd cmake-template
````

### 2. Configure the Project
If you only want to install the project use ``-DBUILD_TESTING=OFF``, as this option will make the build process way faster.

```sh
cmake -S . -B build
````

### 3. Build the Library

```sh
cmake --build build
````

### 4. Run Unit Tests

```sh
cd build
ctest
````

### 5. Install the library

```sh
cd build
sudo make install
````

### 6. Uninstall the library

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

---

## License
This project is a template and does not include a license by default. Add a LICENSE file as appropriate for your use.