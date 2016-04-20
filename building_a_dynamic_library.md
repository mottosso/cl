### Building a Dynamic Library

> Source: [Albertech Blog](http://albertech.blogspot.co.uk/2014/12/how-to-compile-dll-using-clexe.html)

Here's a minimal source and command line for building a Windows dynamic link library, or DLL. I like to use it as a quick template for larger projects, without all the noise you get by creating a similar project in Visual Studio.

```cpp
#include <stdio.h>
#include <wchar.h>

extern "C" {

__declspec(dllexport)
void print_helo(const wchar_t* u) {
    wprintf(L"hello: %s\n", u);
}

} // extern "C"
```

This is very trivial as you can see. It has a single function that expects an array of wide chars, which it outputs.
A couple notes on the source.

`extern "C" {}` informs the compiler that we want the function names to be preserved. That is, to not ["mangle"](https://en.wikipedia.org/wiki/Name_mangling#Simple_example) the names as is done for C++ code.

This way, when we do..

```cmd
dumpbin.exe /exports helloworld.dll
```

..we will see and can call the function name as we typed it:

```cpp
          1    0 00001000 print_helo
```

Also, we use `__declspec(dllexport)` so we do not need to use a `.def` file to export the function.
If the file is saved in `helloworld.cpp`, you would compile it from the command line like so:

```cmd
cl.exe /D_USRDLL /D_WINDLL helloworld.cpp /MT /link /DLL /OUT:helloworld.dll
```

More details about build settings for a DLL can be found here: http://msdn.microsoft.com/en-us/library/aa235516%28v=vs.60%29.aspx
BONUS ROUND
And here is how you'd call print_helo() from Python. (What's the use of a DLL unless you have some way of using it, am I right?) Pretend we have a file helowrld.py in the same directory as our compiled helowrld.dll:

```python
import os
import sys
from ctypes import *

lib = cdll.LoadLibrary('helowrld.dll')

# Our 'ctypes' wrapper around the DLL function -- this is where we
# convert Python types to C types and call the DLL function.
def print_helo(w):
    func = lib.print_helo
    func.argtypes = [c_wchar_p]
    p = c_wchar_p(w)
    func(p, len(w))


print_helo('my name is ...')
```