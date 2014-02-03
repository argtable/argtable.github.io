---
layout: default2
title: Argtable.org
---

## What is Argtable

Argtable is an open source ANSI C library that parses GNU-style command-line options. It simplifies command-line parsing by defining a declarative-style API that you can use to specify *what* your command-line syntax looks like. Argtable will automatically generate consistent error handling logic and textual descriptions of the command line syntax, which are essential but tedious to implement for a robust CLI program. For example, to create a CLI program that looks like:

```
$> util.exe --help
Usage: util.exe [-v] [--help] [--version] [--level=<n>] [-o myfile] <file> [<file>]...
Demonstrate command-line parsing in argtable3.

  --help                    display this help and exit
  --version                 display version information and exit
  --level=<n>               foo value
  -v, --verbose             verbose output
  -o myfile                 output file
  <file>                    input files
```

You can implement the command-line parsing logic with Argtable in the following code snippet:

```cpp
#include "argtable3.h"

/* global arg_xxx structs */
struct arg_lit *verb, *help, *version;
struct arg_int *level;
struct arg_file *o, *file;
struct arg_end *end;

int main(int argc, char *argv[])
{
    /* the global arg_xxx structs are initialised within the argtable */
    void *argtable[] = {
        help    = arg_lit0(NULL, "help", "display this help and exit"),
        version = arg_lit0(NULL, "version", "display version info and exit"),
        level   = arg_int0(NULL, "level", "<n>", "foo value"),
        verb    = arg_lit0("v", "verbose", "verbose output"),
        o       = arg_file0("o", NULL, "myfile", "output file"),
        file    = arg_filen(NULL, NULL, "<file>", 1, 100, "input files"),
        end     = arg_end(20),
    };
    
    int exitcode = 0;
    char progname[] = "util.exe";
    
    int nerrors;
    nerrors = arg_parse(argc,argv,argtable);

    /* special case: '--help' takes precedence over error reporting */
    if (help->count > 0)
    {
        printf("Usage: %s", progname);
        arg_print_syntax(stdout, argtable, "\n");
        printf("Demonstrate command-line parsing in argtable3.\n\n");
        arg_print_glossary(stdout, argtable, "  %-25s %s\n");
        exitcode = 0;
        goto exit;
    }

    /* If the parser returned any errors then display them and exit */
    if (nerrors > 0)
    {
        /* Display the error details contained in the arg_end struct.*/
        arg_print_errors(stdout, end, progname);
        printf("Try '%s --help' for more information.\n", progname);
        exitcode = 1;
        goto exit;
    }

exit:
    /* deallocate each non-null entry in argtable[] */
    arg_freetable(argtable, sizeof(argtable) / sizeof(argtable[0]));
    return exitcode;
}
```


## Features

Here is a list of reasons why you should include Argtable in your C/C++ toolbox:

* **GNU-style command-line syntax**: Use the standard and cross-platform way to express commands.
* **Declarative API**: Eliminate complex parsing logic by specifying *what* instead of elaborating *how*.
* **Built-in error handling**: Generate consistent error handling logic.
* **Built-in help messages**: Generate consistent command-line syntax descriptions.
* **Written in ANSI C**: Easy to create bindings for other languages.
* **Readable source code**: Well-commented source code with 100% branch test coverage.
* **Single-file library**: No tedious build scripts. Just drop a single source file into your projects.
* **Self-contained**: No external dependencies.
* **Cross-platform**: Available on most UNIX-like systems, Windows, and embedded systems.
* **BSD-licensed**: Use the library for any purpose, including in commercial programs.


## What's Next

If you want to learn how to use Argtable3, you can start from the [tutorial][tutorial]. If you want to learn from examples, you can check a list of [sample programs][example] in the repository. If you are already familiar with Argtable3, you can check the [documentation][doc] for API details. And if you find any issue of the documentation or code, you can submit an issue to the [Github project page][issue].

CLI programs have become more and more important in the cloud computing era. We hope that Argtable can contribute to the Renaissance of CLI and help to make both developers' and users' life easier.

<span class="label label-default">NOTICE</span> This web site is for the latest Argtable **v3 series**, which is derived from Argtable v2 series created by [Stewart Heitmann][heitmann]. **Argtable3 is not backward-compatible**. Therefore, if you want to use Argtable2 API, you have to go to the [Argtable2 web site][argtable2] and get the source code from its [Sourceforge.net project page][argtable2-sourceforge].

[heitmann]: email:sheitmann@users.sourceforge.net
[argtable2]: http://argtable.sourceforge.net/
[argtable2-sourceforge]: http://sourceforge.net/projects/argtable/
[tutorial]: http://argtable.org/tutorial/
[example]: http://argtable.org/examples/
[doc]: http://argtable.org/documentation/
[issue]: https://github.com/tomghuang/tomghuang.github.io/issues

