C Macro Overloading by Argument List Length
===========================================

Single header providing one macro allowing overloading of other macros based on
number of passed arguments.

May be used for providing **default argument values for functions**.

# Usage

Choose `prefix` for overloading (may be the same as `macro`), then define:
```c
#define macro(...) CMOBALL(prefix, __VA_ARGS__)
```

Then define macros using `prefix_N(x1,x2,...,xN)` notation (where `N` is number of taken arguments).

## Examples

```c
#include <stdio.h>
#include "cmoball.h"

int foo(int a, int b, int c)
{
    return a + b + c;
}

#define foo(...) CMOBALL(FOO, __VA_ARGS__)
#define FOO_3(x,y,z) foo(x,y,z)
#define FOO_2(x,y)   FOO_3(x,y,2)
#define FOO_1(x)     FOO_2(x,2)
#define FOO_0()      FOO_1(2)

int main()
{
    printf("%d\n", foo());      // 6
    printf("%d\n", foo(1));     // 5
    printf("%d\n", foo(1,1,1)); // 3

    printf("%d\n", bar(4,0));   // 2
    printf("%d\n", bar(4,1,1)); // 3
    // printf("%d\n", bar(1));  // error

    return 0;
}
```

```c
#include <stdio.h>
#include "cmoball.h"

int bar(int x, int b, int h)
{
    return 2+h;
}

#define bar_3(x,y,z) bar(x,y,z)
#define bar(...)     CMOBALL(bar, __VA_ARGS__) // no particular order needed
#define bar_2(x,y)   bar_3(x,y,0)

int main()
{
    printf("%d\n", bar(4,0));   // 2
    printf("%d\n", bar(4,1,1)); // 3
    // printf("%d\n", bar(1));  // error

    return 0;
}
```

```c
#include <stdio.h>
#include "cmoball.h"

#define NoA(...) CMOBALL(FOO, __VA_ARGS__)
#define FOO_3(x,y,z) "Three"
#define FOO_2(x,y)   "Two"
#define FOO_1(x)     "One"
#define FOO_0()      "Zero"


int main()
{
    puts(NoA());
    puts(NoA(1));
    puts(NoA(1,1));
    puts(NoA(1,1,1));
    return 0;
}
```
