This is the full output of the failing configuration when vcpkg *is* integrated for Visual Studio, ie. `C:\Code\vcpkg\vcpkg integrate install` and the project is built for MSBuild/Clang:

```powershell
rm -r -fo buildMSBuildClang; 
cmake -G"Visual Studio 16 2019" -T ClangCl -A Win32  -S . -B buildMSBuildClang `
  -D CMAKE_TOOLCHAIN_FILE="C:\Code\vcpkg\scripts\buildsystems\vcpkg.cmake"

cmake --build .\buildMSBuildClang\ --config Release
.\buildMSBuildClang\Release\fibo.exe
```

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
Restored 5 package(s) from C:\Users\philippeqc\AppData\Local\vcpkg\archives in 398.5 ms. Use --debug to see more details.
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
See also "C:/Users/philippeqc/source/repos/vcpkg/fibo/buildMSBuildClang/CMakeFiles/CMakeOutput.log".
See also "C:/Users/philippeqc/source/repos/vcpkg/fibo/buildMSBuildClang/CMakeFiles/CMakeError.log".
```
