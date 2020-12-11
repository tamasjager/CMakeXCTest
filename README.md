# CMakeXCTest
Sample project to demonstrate linker error when using CMake 3.19.

Information about the issue and its workaround (which stopped working since v3.19) can be found [here](https://gitlab.kitware.com/cmake/cmake/-/issues/19809).

Create the project with
```
cmake . -B ./build/cmake -DCMAKE_SYSTEM_NAME=iOS -G Xcode
```
then try running the tests from Xcode (tested with 12.2).
- Choose the `TestHost` scheme.
- Choose a simulator (iPhone or iPad) as the destination.
- `Cmd + U` to run the tests.

You'll get linker errors due to the framework search path being set to the `iPhoneOS` version of the `XCTest` fw instead of the simulator.
