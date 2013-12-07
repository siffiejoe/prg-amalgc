#                             prg-amalgc                             #

A perl script that creates a single C source file containing all files
that are `#include`d via C preprocessor directives.

E.g. SQLite uses an [amalgamated source file][1] for easier
distribution and more effective optimization.

  [1]: http://www.sqlite.org/amalgamation.html

Run `amalgc --help` for usage information.


##                              Example                             ##

There has been interest in the Lua mailing list about a version of
[Lua][2], which is distributed as a single C source file. LHF, one
of Lua's creators, posted a [file][3] that can be used to create such
an amalgamated source file.

1. Download the referenced file as `one.c` and put it into the `src`
   directory of Lua's source code.
2. Run `amalgc -i . -l -o amalglua.c one.c` to create a file
   `amalglua.c` which includes all the sources for the Lua library,
   the standalone interpreter, and the `luac` bytecode compiler.
3. Use the appropriate compiler incantation to compile the file. (E.g.
   to compile the standalone interpreter on my Linux box use:

```
gcc -O2 -Wall -DLUA_COMPAT_ALL -DLUA_USE_LINUX -o amalglua amalglua.c -Wl,-E -lm -ldl -lreadline
```

  [2]: http://www.lua.org/
  [3]: http://lua-users.org/lists/lua-l/2011-12/msg00249.html


##                              Caveats                             ##

The heuristic used to create the amalgamated source file is not
perfect. In particular, it will fail for conditional inclusion of
source files, like e.g.:

```C
#ifdef SOME_DEFINE
#include "file.h"
#endif

/* ... */

#include "file.h"
```

The first `#include` will be replaced by the file contents, the second
will just be ignored, so the resulting source file will only compile
if `SOME_DEFINE` is `#define`d.


##                              License                             ##

    Copyright (C) 2011  Philipp Janda
    
    This program is free software: you can redistribute it and/or modify
    it under the terms of the GNU General Public License as published by
    the Free Software Foundation, either version 3 of the License (see
    below), or (at your option) any later version.
    
    This program is distributed in the hope that it will be useful,
    but WITHOUT ANY WARRANTY; without even the implied warranty of
    MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
    GNU General Public License for more details.


