# Chapter 2: Setting up your Environment (Interview Revision)

## Core Concepts

### Installing C++

#### Compilers
- **GCC (GNU Compiler Collection)**: Linux, cross-platform
- **Clang**: Modern compiler, better error messages
- **MSVC (Microsoft Visual C++)**: Windows, Visual Studio
- **MinGW**: GCC for Windows

#### Installation Commands
```bash
# Ubuntu/Debian
sudo apt update
sudo apt install build-essential gdb

# CentOS/RHEL
sudo yum groupinstall "Development Tools"

# macOS (with Xcode Command Line Tools)
xcode-select --install

# Windows
# Download MinGW or Visual Studio Community
```

### Code Editors/IDEs

#### Professional IDEs
- **Visual Studio**: Windows, best for C++ development
- **CLion**: Cross-platform, paid but excellent
- **Xcode**: macOS, Apple development

#### Lightweight Editors
- **VS Code**: Free, excellent extensions
- **Sublime Text**: Fast, customizable
- **Vim/Neovim**: Terminal-based, powerful

#### Key VS Code Extensions
- C/C++ (Microsoft)
- C/C++ Compile Run
- C/C++ Extension Pack
- Code Runner

## Build Process (Interview Important)

### Compilation Stages
1. **Preprocessing**: `#include`, `#define`, macro expansion
2. **Compilation**: Source → Assembly code
3. **Assembly**: Assembly → Object files (.o/.obj)
4. **Linking**: Object files → Executable

### Basic Compilation Commands
```bash
# Single file
g++ -o program program.cpp

# Multiple files
g++ -o program main.cpp utils.cpp

# With optimizations
g++ -O2 -o program program.cpp

# Debug build
g++ -g -o program program.cpp

# All warnings enabled
g++ -Wall -Wextra -o program program.cpp
```

### Makefile Basics
```makefile
# Variables
CXX = g++
CXXFLAGS = -Wall -Wextra -std=c++17
TARGET = program
SOURCES = main.cpp utils.cpp

# Rules
all: $(TARGET)

$(TARGET): $(SOURCES)
	$(CXX) $(CXXFLAGS) -o $(TARGET) $(SOURCES)

clean:
	rm -f $(TARGET)

.PHONY: all clean
```

## Key Interview Points

### Must-Know Concepts
- **Compilation process**: Preprocess → Compile → Assemble → Link
- **Debug vs Release builds**: -g vs -O flags
- **Static vs Dynamic linking**: .a/.lib vs .so/.dll
- **Header files**: Declarations, not definitions
- **Build systems**: Make, CMake, Ninja

### Common Interview Questions

### Q1: Explain the C++ compilation process
**Answer:**
1. **Preprocessing**: Handles `#include`, `#define`, conditional compilation
2. **Compilation**: Converts preprocessed code to assembly
3. **Assembly**: Converts assembly to machine code (object files)
4. **Linking**: Combines object files and libraries into executable

### Q2: What's the difference between .h and .cpp files?
**Answer:**
- **.h files**: Declarations (function signatures, class definitions)
- **.cpp files**: Implementations (function bodies, definitions)
- Headers can be included multiple times, implementations only once
- Prevent multiple inclusion with include guards

### Q3: What are static and dynamic libraries?
**Answer:**
- **Static libraries (.a/.lib)**: Linked at compile time, part of executable
  - Pros: No dependencies at runtime
  - Cons: Larger executable size
- **Dynamic libraries (.so/.dll)**: Linked at runtime
  - Pros: Smaller executable, shared memory
  - Cons: Dependency management

### Q4: What's the difference between -O1, -O2, -O3?
**Answer:**
- **-O1**: Basic optimizations, safe for all programs
- **-O2**: Aggressive optimizations, recommended for release
- **-O3**: Maximum optimizations, may increase code size
- **-O0**: No optimization, default for debug builds

## Code Examples

### Include Guards (Critical Concept)
```cpp
// ❌ Bad: No include guard
// utils.h
void print_message();

// ❌ Problem: Multiple inclusion causes errors
#include "utils.h"
#include "utils.h"  // Error: redefinition

// ✅ Good: Include guards
#ifndef UTILS_H
#define UTILS_H

void print_message();

#endif  // UTILS_H

// ✅ Modern C++: #pragma once
#pragma once
void print_message();
```

### Basic Program Structure
```cpp
#include <iostream>     // Standard library
#include "utils.h"      // Local header

int main() {
    std::cout << "Hello World!" << std::endl;
    return 0;           // Success exit code
}
```

### Multi-file Project
```cpp
// main.cpp
#include <iostream>
#include "calculator.h"

int main() {
    Calculator calc;
    int result = calc.add(5, 3);
    std::cout << "Result: " << result << std::endl;
    return 0;
}
```

```cpp
// calculator.h
#pragma once

class Calculator {
public:
    int add(int a, int b);
    int subtract(int a, int b);
};
```

```cpp
// calculator.cpp
#include "calculator.h"

int Calculator::add(int a, int b) {
    return a + b;
}

int Calculator::subtract(int a, int b) {
    return a - b;
}
```

## Common Mistakes & Solutions

### Mistake 1: Missing include guards
```cpp
// ❌ Bad: No include guard
// header.h
class MyClass { ... };

// ✅ Good: Include guard or pragma once
#pragma once
class MyClass { ... };
```

### Mistake 2: Defining functions in headers
```cpp
// ❌ Bad: Function definition in header (causes linking errors)
// header.h
void myFunction() {
    // Implementation
}

// ✅ Good: Declaration only in header
void myFunction();  // Declaration

// ✅ Good: Definition in .cpp file
void myFunction() {
    // Implementation
}
```

### Mistake 3: Not using namespaces properly
```cpp
// ❌ Bad: using namespace in headers
// header.h
using namespace std;  // Affects all files that include this header

// ✅ Good: Use std:: prefix in headers
#include <string>
std::string getName();
```

## Build Systems Comparison

### Make vs CMake
| Feature | Make | CMake |
|---------|------|-------|
| Syntax | Makefile syntax | CMakeLists.txt syntax |
| Cross-platform | ❌ Limited | ✅ Excellent |
| IDE integration | ❌ Basic | ✅ Excellent |
| Learning curve | 📈 Medium | 📈 Steeper |

### Basic CMake Example
```cmake
# CMakeLists.txt
cmake_minimum_required(VERSION 3.10)
project(MyProject)

# Set C++ standard
set(CMAKE_CXX_STANDARD 17)

# Add executable
add_executable(myapp main.cpp calculator.cpp)

# Include directories
target_include_directories(myapp PRIVATE ${CMAKE_CURRENT_SOURCE_DIR})
```

## Quick Reference Commands

### Compilation Flags
```bash
-g              # Debug information
-O0, -O1, -O2, -O3  # Optimization levels
-Wall           # All warnings
-Wextra         # Extra warnings
-std=c++11/14/17/20  # C++ standard
-pedantic       # Strict standard compliance
```

### Useful Commands
```bash
# Check compiler version
g++ --version

# Create object files
g++ -c main.cpp

# Link object files
g++ -o program main.o utils.o

# Clean build files
make clean

# Run with debugging
gdb ./program
```

## Memory & Performance Tips

### Debug vs Release Builds
```bash
# Debug build (development)
g++ -g -Wall -Wextra -O0 -o debug_program program.cpp

# Release build (production)
g++ -O2 -DNDEBUG -o release_program program.cpp
```

### Optimization Examples
```cpp
// Compiler can optimize this better
int result = x * 2;  // Becomes: x << 1

// Loop unrolling happens with -O2+
for (int i = 0; i < 4; i++) {
    array[i] = 0;
}
// Becomes: array[0]=0; array[1]=0; array[2]=0; array[3]=0;
```

## Final Interview Tips

1. **Know the compilation process** - 4 stages are critical
2. **Understand include guards** - essential interview concept
3. **Know debug vs release builds** - -g vs -O flags
4. **Understand static vs dynamic linking** - common interview topic
5. **Remember header vs source file roles** - declarations vs definitions
6. **Know basic build systems** - Make and CMake fundamentals

---

**Remember**: Environment setup is foundational - interviewers want to see you understand how C++ code becomes executable programs!