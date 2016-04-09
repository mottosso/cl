### Building a library

Starting from the bottom up, we'll build a library and then use that library in the following examples.

We will have 3 files in this layout.

```
| Layout              | Description
|---------------------|----------------------------------
| root/               |
|   includes/         |
|     lib.h           | Definition of a sample function
|   src/              |
|     lib.cpp         | Declaration of function
|     main.cpp        | Use of library
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

### Compile

```bat
@echo off
pushd build

cl^
  -Zi^
  -EHsc^
  -c^
  -I "..\include"^
  ..\src\lib.cpp &&^
lib^
  lib.obj

popd

```