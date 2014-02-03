---
layout: default2
title: Argtable.org
---

## What is Argtable

Argtable is an open source ANSI C library that parses GNU-style command-line options. It simplifies command-line parsing by defining a declarative-style API that you use to specify <b>what</b> your command-line options look like. Artable will automatically generate consistent error handler and online help, which are essential but tedious to implement for a robust CLI program. For example, to create a CLI program that looks like:

```
$> ilmerge /target:winexe /out:..\crvw.exe crvw.exe crvcore.dll dotnetzip.dll
```

You can implement with Argtable in the following code snippet:

```cpp
#include <argtable2.h>

struct arg_lit *a, *b, *c, *verb;
struct arg_int *scal;
struct arg_file *o, *file;
struct arg_end *end;
int main(int argc, char **argv)
{
    /* the global arg_xxx structs are initialised within the argtable */
    void *argtable[] = {
        a = arg_lit0("a", NULL, "the -a option"),
        b = arg_lit0("b", NULL, "the -b option"),
        c = arg_lit0("c", NULL, "the -c option"),
        scal = arg_int0(NULL, "scalar","<n>", "foo value"),
        verb = arg_lit0("v", "verbose, "verbose output"),
        o = arg_file0("o", NULL,"myfile", "output file"),
        file = arg_filen(NULL,NULL,"<file>",1,2, "input files"),
        end = arg_end(20),
    };

    return 0;
};
```

If you want to create useful utilities in .NET Platform but hate this tons-of-assemblies phenomenon, you can use `ILMerge`. `ILMerge` is a tiny utility from Microsoft Research, which can link all of your application assemblies into a single-file executable. For example, when I ported my code review tool to C#, I ended up having three assemblies: `crvw.exe`, `crvcore.dll`, and `dotnetzip.dll`. In the last step in my build script, I use the following command to merge all these assemblies into a single `crvw.exe`:


## Features

* **GNU-style command-line syntax**: A standard, cross-platform way to express command-line options.
* **Declarative API**: Eliminate complex parsing logic by specifying *what* instead of *how*.
* **Built-in error handler**: Generate consistent error handling logic.
* **Built-in help messages**: Generate consistent documentation.
* **Written in ANSI-C**: Easy to create bindings for other languages.
* **Readable source code**: Well-commented source code with 100% branch test coverage.
* **Single-file library**: No tedious build scripts. Just drop a single source file into your projects.
* **Self-contained**: No external dependencies.
* **Cross-platform**: Available on UNIX (Linux, Mac OS-X, Android, iOS) and Windows.
* **BSD-licensed**: Use the library for any purpose, including in commercial programs.

