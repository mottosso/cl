### Building a library

Starting from the bottom up, we'll build a library and then use that library in the next example.

We will have 3 files in this layout.

```
| Layout              | Description
|---------------------|----------------------------------
| root/               |
|   includes/         |
|     lib.h           | 1. Definition of a sample function
|   src/              |
|     lib.cpp         | 2. Declaration of function
|     main.cpp        | 3. Use of library
|   build/            |
|     ...             | Output goes here
```

Here is the content of each file.

**lib.h**

```cpp
namespace mynamespace
{
    void helloWorld();
}
```

**lib.cpp**

```cpp
#include <iostream>
#include "lib.h"

namespace mynamespace
{
    void helloWorld()
    {
        std::cout << "Hello World!" << std::endl;
    }
}
```

**main.cpp**

```cpp
#include "lib.h"

int main(void)
{
    mynamespace::helloWorld();
    return(0);
}
```

<br>
<br>
<br>

#### Compile Library

Before compiling anything, you will need to setup your environment.

**term.bat**

```bat
set PATH=C:\Program Files (x86)\Microsoft Visual Studio 14.0\Common7\IDE;%PATH%
call "c:\Program Files (x86)\Microsoft Visual Studio 12.0\VC\vcvarsall.bat" x86
```

This will output `build/lib.lib`.

```bat
@echo off
pushd build

cl^
  -Zi^
  -EHsc^
  -I "..\include"^
  -c^
  ..\src\lib.cpp &&^
lib^
  lib.obj

popd
```

#### Compile Program

This will use `build/lib.lib` to compile a console application.

Notice the similarities between compilation, and differing second command; from `lib.exe` to `link.exe`.

```bat
@echo off
pushd build

cl^
  -Zi^
  -EHsc^
  -I "..\include"^
  -c^
  ..\src\main.cpp &&^
link main.obj^
  lib.lib^
  /LIBPATH:"..\build"

popd
```