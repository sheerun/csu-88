# csu-88

Source code of Csu (C startup). The first commit in this repository represents has code imported from https://opensource.apple.com/source/Csu/Csu-88/ as of 2020-06-14.

All next commits are created in order to compile it under MacOS Catalina.

## Usage

Compile crt0.o which can be used for statically linked binary on MacOS:

```
make
```
 
Compile all files supported by this source (e.g. `crt1.o` which is used to load dynamically linked programs):

```
make all
```

## Can you use it for statically linking binary?

Yes and no.

Per documentation for `-static` linking flag:

> This option will not work on Mac OS X unless all libraries (including libgcc.a) have also been compiled with -static. Since neither a static version of libSystem.dylib nor crt0.o are provided, this option is not useful to most people.

It means that static version of `crt0.o` is not enough. There is no official version of libSystem on MacOS (which is an equivalent of libgcc) other than libSystem.dylib, which is a dynamic library. And that means there is no static version of libSystem for crt0.o to link with, which causes the errors about missing functions.

There is a source of libSystem available to [download](https://opensource.apple.com/tarballs/Libsystem/Libsystem-1281.tar.gz), but I do not know whether it's possible to statically compile it. If you even manage to do it, then you can use command like `ld main.o -static -L${PWD} -lcrt0.o -llibSystem.o -e _main -o main` to produce static binary for MacOS
