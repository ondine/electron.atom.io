---
version: v1.4.2
category: Development
redirect_from:
    - /docs/v0.24.0/development/setting-up-symbol-server/
    - /docs/v0.25.0/development/setting-up-symbol-server/
    - /docs/v0.26.0/development/setting-up-symbol-server/
    - /docs/v0.27.0/development/setting-up-symbol-server/
    - /docs/v0.28.0/development/setting-up-symbol-server/
    - /docs/v0.29.0/development/setting-up-symbol-server/
    - /docs/v0.30.0/development/setting-up-symbol-server/
    - /docs/v0.31.0/development/setting-up-symbol-server/
    - /docs/v0.32.0/development/setting-up-symbol-server/
    - /docs/v0.33.0/development/setting-up-symbol-server/
    - /docs/v0.34.0/development/setting-up-symbol-server/
    - /docs/v0.35.0/development/setting-up-symbol-server/
    - /docs/v0.36.0/development/setting-up-symbol-server/
    - /docs/v0.36.3/development/setting-up-symbol-server/
    - /docs/v0.36.4/development/setting-up-symbol-server/
    - /docs/v0.36.5/development/setting-up-symbol-server/
    - /docs/v0.36.6/development/setting-up-symbol-server/
    - /docs/v0.36.7/development/setting-up-symbol-server/
    - /docs/v0.36.8/development/setting-up-symbol-server/
    - /docs/v0.36.9/development/setting-up-symbol-server/
    - /docs/v0.36.10/development/setting-up-symbol-server/
    - /docs/v0.36.11/development/setting-up-symbol-server/
    - /docs/v0.37.0/development/setting-up-symbol-server/
    - /docs/v0.37.1/development/setting-up-symbol-server/
    - /docs/v0.37.2/development/setting-up-symbol-server/
    - /docs/v0.37.3/development/setting-up-symbol-server/
    - /docs/v0.37.4/development/setting-up-symbol-server/
    - /docs/v0.37.5/development/setting-up-symbol-server/
    - /docs/v0.37.6/development/setting-up-symbol-server/
    - /docs/v0.37.7/development/setting-up-symbol-server/
    - /docs/v0.37.8/development/setting-up-symbol-server/
    - /docs/latest/development/setting-up-symbol-server/
source_url: 'https://github.com/electron/electron/blob/master/docs/development/setting-up-symbol-server.md'
title: "Setting Up Symbol Server in Debugger"
sort_title: "setting up symbol server in debugger"
---

# Setting Up Symbol Server in Debugger

Debug symbols allow you to have better debugging sessions. They have information
about the functions contained in executables and dynamic libraries and provide
you with information to get clean call stacks. A Symbol Server allows the
debugger to load the correct symbols, binaries and sources automatically without
forcing users to download large debugging files. The server functions like
[Microsoft's symbol server](http://support.microsoft.com/kb/311503) so the
documentation there can be useful.

Note that because released Electron builds are heavily optimized, debugging is
not always easy. The debugger will not be able to show you the content of all
variables and the execution path can seem strange because of inlining, tail
calls, and other compiler optimizations. The only workaround is to build an
unoptimized local build.

The official symbol server URL for Electron is
http://54.249.141.255:8086/atom-shell/symbols.
You cannot visit this URL directly, you must add it to the symbol path of your
debugging tool. In the examples below, a local cache directory is used to avoid
repeatedly fetching the PDB from the server. Replace `c:\code\symbols` with an
appropriate cache directory on your machine.

## Using the Symbol Server in Windbg

The Windbg symbol path is configured with a string value delimited with asterisk
characters. To use only the Electron symbol server, add the following entry to
your symbol path (**Note:** you can replace `c:\code\symbols` with any writable
directory on your computer, if you'd prefer a different location for downloaded
symbols):

```
SRV*c:\code\symbols\*http://54.249.141.255:8086/atom-shell/symbols
```

Set this string as `_NT_SYMBOL_PATH` in the environment, using the Windbg menus,
or by typing the `.sympath` command. If you would like to get symbols from
Microsoft's symbol server as well, you should list that first:

```
SRV*c:\code\symbols\*http://msdl.microsoft.com/download/symbols;SRV*c:\code\symbols\*http://54.249.141.255:8086/atom-shell/symbols
```

## Using the symbol server in Visual Studio

<img src='http://mdn.mozillademos.org/files/733/symbol-server-vc8express-menu.jpg'>
<img src='http://mdn.mozillademos.org/files/2497/2005_options.gif'>

## Troubleshooting: Symbols will not load

Type the following commands in Windbg to print why symbols are not loading:

```
> !sym noisy
> .reload /f electron.exe
```
