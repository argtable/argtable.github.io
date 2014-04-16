---
layout: post2
title:  "A Tutorial Introduction of Argtable3"
date:   2014-04-09 10:12:04
tags: tutorial
---

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


## Acknowledgements

* *Stewart Heitmann* for creating the original argtable-1.x and argtable-2.x libraries.
* *Nina Clemson* for editing the original argtable-1.0 documentation.
* *Livio Bertacco* for contributing bug fixes and the argtable-2.x Visual C++ Makefiles.
* *Justin Dearing* for contributing bug fixes and Windows DLL support, plus code support for the Open Watcom compiler and help with the Mac OS X configuration.
* *Asa Packer* for contributing bug fixes and upgrades to the Visual C++ Makefiles.
* *Danilo Cicerone* for the Italian translation of "Introduction to Argtable-2x" and the argtable devpak.
* *Uli Fouquet* for configuration patches and documentation related to cross-compiling argtable from Unix to Windows as well as providing the arg_print_glossary_gnu function.
* *Shachar Schemesh* for integrating argtable into Debian Linux and kick-starting the migration to automake/autoconf.
* *Jasper Lievisse Adriaanse* for maintaining the argtable package in OpenBSD ports.
* *Ulrich Mohr* for bug fixes relating to Texas Instrument DSP platforms.
* *John Vickers* and *Steve O'Neil* for bug fixes relating to Solaris/Motorola platforms.
* *Lori A. Pritchett-Sheats* for fixing a makefile bug relating to "make dist".
* *Paolo Bormida* for instructions on building argtable with date and regex support on Windows.
* *Michel Valin* for bug fixes relating to the configure scripts on IBM AIX platforms and instructions on compiling the example code under AIX.
* *Steve Christensen* for providing prebuilt packages for SPARC/Solaris and x86/Solaris platforms on www.sunfreeware.com.
* *Jess Portnoy* for reworking the rpm package and integrating argtable into Fedora Linux.
* *Michael Brown* for incorporating support for pkg-config into the autoconf scripts.
* *Alexander Lindert* for extensions to the parser to support hex, octal and binary integer formats as well as KB/MB/GB suffixes.
* *Rob Zaborowski* for providing build configuration files for the CMake tool.
* *Moczik Gabor* for bug fixes relating to the parsing of filepaths and filename extensions.
* *Hanspeter Niederstrasser* for including the argtable package in FinkProject for Mac OS X.

[heitmann]: email:sheitmann@users.sourceforge.net
[argtable2]: http://argtable.sourceforge.net/
[argtable2-sourceforge]: http://sourceforge.net/projects/argtable/
[tutorial]: http://argtable.org/tutorial/
[example]: http://argtable.org/examples/
[doc]: http://argtable.org/documentation/
[issue]: https://github.com/tomghuang/tomghuang.github.io/issues

