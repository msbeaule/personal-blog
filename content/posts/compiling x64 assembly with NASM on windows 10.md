---
title: "Compiling x64 Assembly with NASM on Windows 10"
aliases: compiling assembly with Visual Studio 2022 Tools x64 and NASM on windows 10
date: 2024-03-08T05:10:00
draft: false
tags: ["windows", "x64", "assembly", "nasm"]
categories: ["how to"]
description: "Using Visual Studio 2022 and NASM you can compile x64 assembly into an executable file."
# lastmod: 

uniqueHash: "3742126959577630"

# comments: true
# weight: 1
# aliases: ["/first"]
# author: ""
# showToc: true
# TocOpen: false
# hidemeta: false

# disableShare: false
# hideSummary: false
# searchHidden: true
# ShowBreadCrumbs: true
# ShowPostNavLinks: true
# ShowWordCount: true
# ShowRssButtonInSectionTermList: true
# UseHugoToc: true

---

I was following [this very good tutorial and introduction to assembly](https://sonictk.github.io/asm_tutorial/), but I was encountering errors running the hello world example. It may be due to differences with Visual Studio 2017 and 2022, or maybe my path environment variable wasn't set up right.

Below is a guide to compile assembly with VS 2022 and NASM, if the instructions in the above tutorial didn't work for you.

## Prerequisites

- A computer running Windows 10
- Install [NASM](https://www.nasm.us/) _(the [repo with the source code for it](https://github.com/netwide-assembler/nasm))_
	- Make sure the folder `nasm.exe` is in, is added to your path
- Install Visual Studio 2022
- Check these folders exist (might be a **different version number** than I have here), and add them to your path:
	- `C:\Program Files (x86)\Windows Kits\10\Lib\10.0.22000.0\um\x64`
	- `C:\Program Files (x86)\Windows Kits\10\Lib\10.0.22000.0\ucrt\x64`

## Steps

Create a folder, and inside that folder make a file, `hello_world.asm`:

```asm
bits 64
default rel

segment .data
    msg db "Hello world!", 0xd, 0xa, 0

segment .text
global main
extern ExitProcess

extern printf

main:
    push    rbp
    mov     rbp, rsp
    sub     rsp, 32

    lea     rcx, [msg]
    call    printf

    xor     rax, rax
    call    ExitProcess
```

Make a `link.bat` file with this (the full path to `link.exe` might vary slightly):

```bat
call "C:\Program Files\Microsoft Visual Studio\2022\Community\VC\Tools\MSVC\14.36.32532\bin\Hostx64\x64\link.exe" hello_world.obj /subsystem:console /entry:main /out:hello_world_basic.exe kernel32.lib ucrt.lib legacy_stdio_definitions.lib
```

{{< notice info >}}
If you already have the Visual Studio x64 `link.exe` in the path already, then you can just use `link` instead of the entire path. In my case, I had Cygwin and MinGW installed and on my path, and it was using those `link`s instead.
{{< /notice >}}

Open Windows search by pressing the Windows key, then search for `x64 Native Tools`, run that Command Prompt.

Run:

```cmd
nasm -f win64 hello_world.asm
```

Then run:

```cmd 
link.bat
```

{{< notice info >}}
The `link` command in our `link.bat` file must be run from the x64 Native Tools Command Prompt, but the NASM command above that can be run in a regular command prompt.
{{< /notice >}}

If these commands worked, it might have a warning, but an `.exe` should be created.

Run the `.exe` from the command line to see if `Hello world!` appears.

## References

Stack Overflow: [How to run x64 Native Tools](https://stackoverflow.com/questions/61209155/how-do-i-get-the-x64-native-tools-developer-command-prompt-for-visual-studio-com)

Stack Overflow: [Unresolved external symbol printf in Windows x64 Assembly Programming with NASM](https://stackoverflow.com/questions/64413414/unresolved-external-symbol-printf-in-windows-x64-assembly-programming-with-nasm)

Reddit: [How to fix error LNK2001: unresolved external symbol printf?](https://www.reddit.com/r/Assembly_language/comments/10qb40p/how_to_fix_error_lnk2001_unresolved_external/)

Stack Overflow: [Error in Assembly linker 'link: extra operand'](https://stackoverflow.com/questions/19677870/error-in-assembly-masm-linker-link-extra-operand)

Possible [Windows SDK files](https://developer.microsoft.com/en-us/windows/downloads/sdk-archive/), maybe the Windows Kits folder
