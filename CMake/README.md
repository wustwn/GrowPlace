1. how to judge the host arc

if("amd64" IN_LIST CMAKE_CPU_ARCHITECTURES) # I am targeting 64-bit x86.
...
if("x86" IN_LIST CMAKE_CPU_ARCHITECTURES) # I am targeting 32-bit x86.



2.  set generator platform
set ( CMAKE_GENERATOR_PLATFORM "x64" )  OR
cmake -DCMAKE_GENERATOR_PLATFORM=x64
