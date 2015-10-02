DXT compression is an algorithm that is very sensitive to floating point precision.

The CUDA compressors generally produce slightly different results than the CPU compressors. Moreover, different CPU compilers generally produce different results as well. Even the same compiler can produce different outputs as well, depending on the target or the optimization settings. The best way to minimize these differences is setting the compiler floating point unit to single precision.

On Windows this can be achieved as follows:

```
#include <float.h>
...
_controlfp(_PC_24, _MCW_PC);
```

and on Linux and other Unixes:

```
#include <fpu_control.h>
...
_FPU_SETCW(0x7f|_FPU_RC_NEAREST|_FPU_SINGLE);
```