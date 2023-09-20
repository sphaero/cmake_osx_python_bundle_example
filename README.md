# Create OSX Bundle with CMake

On OSX for example:
```
mkdir build
cd build
cmake .. -G Xcode -DPython3_ROOT_DIR=~/Library/Frameworks/Python.framework/Versions/3.9/
cmake --build . --target Debug
cpack -C Debug -V
```
