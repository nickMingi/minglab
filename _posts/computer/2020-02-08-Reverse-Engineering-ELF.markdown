---
layout: "post"
title: "Reverse Engineering ELF"
author: "mingi hong"
date: 2020-02-08 13:23:45 -0700
categories: "Computer ReverseEngineering"
permalink: /:categories/:title.html
---

## ELF(Executable and Linker File)

You can use readelf in linux and binutils in macos

1. Every file has header file
2. Download readelf or binutils(macos)

# Command Line
- readelf command for linux
- greadelf command for macos
- export PATH=$PATH:/usr/local/opt/binutils/bin/

1. greadelf -a : dumps everything about the file
2. greadelf -h : dump the ELF header fields
3. greadelf --sections --wide : dump the sections
4. greadelf --segments --wide : dump the segments
5. greadelf -x : dump contents of section

# Tools
1. ghidra
    - download ghidra and add environment variables
    - export PATH=$PATH:~/ghidra_9.1.1_PUBLOC/
    - ghidraRun (running)

# File Headers and binary structures
- Practical Binary Analysis: Build your own linux tools for binary instrumentation

ELF -> Executable and Linker format

ELF Header
- Always at the top of the binary

Portable Executable(PE)
- Windows format
- Core is the same as ELF
    - Header containing information
- BIG differences
    - No segments
    - DOS header

Relative Virtual Address
- A way to reference addresses within binary
- Needed pieces
    - Start with base value(0x010)
        - Represents 'beginning' of binary on disk
    - Offset into binary (0x50): RVA
        - Represents a section, table header, or something of other significance
        - 0x50 bytes into file relative to base value
- Calculate virtual address
    - Base value + offset(RVA) = virtual address

# Analysis on Binary

Static vs Dynamic

- Static: Not executing binary, can save time
    - It is read out (readelf)
    - strings (useful and powerful)
    - xxd
    - file (give us information of binary)
        - debugging information as stripped
    - Ghidra (gives you general flow)
![ghidralaunch](/minglab/assets/ghidralaunch.png)
- Dynamic: You run the binary, investigating
    - GDB is executing program
    - cuckoo sandbook
    - Peda or GEF give you a lot of information with GDB

Interesting things/questions to answer
- type                  : ???
- architecture          : ???
- 64/32                 : ???
- calling convention    : ???
- starting address      : ???
- main                  : ???

main analysis
- functions called ?
- stack space ?

1. using file
- file output
output: ELF 32-bit LSB pie executable, Intel 80386, version 1 (SYSV), dynamically linked, interpreter /lib/ld-linux.so.2, for GNU/Linux 3.2.0, BuildID[sha1]=c6af3a994e1255405e9fab0b970d5071ea33a668, not stripped
- history | grep "*output"
