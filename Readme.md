# Requirement
TLDR: as admin, run `choco install cmake ninja llvm -y` and then install [vcpkg](https://vcpkg.io/en/index.html)

1. LLVM/clang is installed and available from `C:\Program Files\LLVM\bin\`.
1. vcpkg is intalled in `C:\Code\vcpkg`,

# Run
## Will succed

When vcpkg is *not* integrated for Visual Studio, ie. `C:\Code\vcpkg\vcpkg integrate remove`

### Build using Ninja and CLang
```powershell
rm -r -fo buildNinjaClang; 
cmake -GNinja  -S . -B buildNinjaClang `
  -DCMAKE_BUILD_TYPE:STRING=Release `
  "-DCMAKE_C_COMPILER:FILEPATH=C:\Program Files\LLVM\bin\clang.exe" `
  "-DCMAKE_CXX_COMPILER:FILEPATH=C:\Program Files\LLVM\bin\clang++.exe" `
  "-DCMAKE_RC_COMPILER:FILEPATH=C:\Program Files\LLVM\bin\llvm-rc.exe" `
  "-DCMAKE_TOOLCHAIN_FILE=C:\Code\vcpkg\scripts\buildsystems\vcpkg.cmake"

cmake --build .\buildNinjaClang\ --target all
.\buildNinjaClang\fibo.exe
```

Will output:
```powershell
fib(1) = 1
fib(2) = 1
fib(3) = 2
fib(4) = 3
fib(5) = 5
fib(6) = 8
fib(7) = 13
fib(8) = 21
fib(9) = 34
fib(10) = 55
```

### Build using MSBuild and CLang

```powershell
rm -r -fo buildMSBuildClang; 
cmake -G"Visual Studio 16 2019" -T ClangCl -A Win32  -S . -B buildMSBuildClang `
  -D CMAKE_TOOLCHAIN_FILE="C:\Code\vcpkg\scripts\buildsystems\vcpkg.cmake"

cmake --build .\buildMSBuildClang\ --config Release
.\buildMSBuildClang\Release\fibo.exe
```

Will output:
```powershell
fib(1) = 1
fib(2) = 1
fib(3) = 2
fib(4) = 3
fib(5) = 5
fib(6) = 8
fib(7) = 13
fib(8) = 21
fib(9) = 34
fib(10) = 55
```

## Will fail

When vcpkg *is* integrated for Visual Studio, ie. `C:\Code\vcpkg\vcpkg integrate install`

### Build using Ninja and CLang
Will do the same as above.

### Build using MSBuild and CLang

Configuring with cmake output the following and the project is not generated.
```powershell
-- Running vcpkg install
Detecting compiler hash for triplet x64-windows...
Detecting compiler hash for triplet x86-windows...
The following packages will be built and installed:
    cxxopts[core]:x86-windows -> 3.0.0
    fmt[core]:x86-windows -> 9.1.0
    range-v3[core]:x86-windows -> 0.12.0
  * vcpkg-cmake[core]:x64-windows -> 2022-10-30
  * vcpkg-cmake-config[core]:x64-windows -> 2022-02-06#1
Additional packages (*) will be modified to complete this operation.
Restored 5 package(s) from C:\Users\pvilleneuve\AppData\Local\vcpkg\archives in 398.5 ms. Use --debug to see more details.
Installing 1/5 vcpkg-cmake-config:x64-windows...
Elapsed time to handle vcpkg-cmake-config:x64-windows: 21.94 ms
Installing 2/5 vcpkg-cmake:x64-windows...
Elapsed time to handle vcpkg-cmake:x64-windows: 65.3 ms
Installing 3/5 cxxopts:x86-windows...
Elapsed time to handle cxxopts:x86-windows: 44.27 ms
Installing 4/5 fmt:x86-windows...
Elapsed time to handle fmt:x86-windows: 157.5 ms
Installing 5/5 range-v3:x86-windows...
Elapsed time to handle range-v3:x86-windows: 328.3 ms

Total elapsed time: 8.969 s
cxxopts provides CMake targets:

    # this is heuristically generated, and may not be correct
    find_package(cxxopts CONFIG REQUIRED)
    target_link_libraries(main PRIVATE cxxopts::cxxopts)

The package fmt provides CMake targets:

    find_package(fmt CONFIG REQUIRED)
    target_link_libraries(main PRIVATE fmt::fmt)

    # Or use the header-only version
    find_package(fmt CONFIG REQUIRED)
    target_link_libraries(main PRIVATE fmt::fmt-header-only)

range-v3 provides CMake targets:

    # this is heuristically generated, and may not be correct
    find_package(range-v3 CONFIG REQUIRED)
    # note: 2 additional targets are not displayed.
    target_link_libraries(main PRIVATE range-v3 range-v3-meta range-v3::meta range-v3-concepts)

-- Running vcpkg install - done
-- Selecting Windows SDK version 10.0.19041.0 to target Windows 10.0.19044.
-- The CXX compiler identification is unknown
CMake Error at CMakeLists.txt:3 (project):
  No CMAKE_CXX_COMPILER could be found.



-- Configuring incomplete, errors occurred!
See also "C:/Users/pvilleneuve/source/repos/vcpkg/fibo/buildMSBuildClang/CMakeFiles/CMakeOutput.log".
See also "C:/Users/pvilleneuve/source/repos/vcpkg/fibo/buildMSBuildClang/CMakeFiles/CMakeError.log".
```

# Conclusion
Even if vcpkg is integrated (for MSBuild), one can still build projects with CLang, so long it isn't as a MSBuild project.
