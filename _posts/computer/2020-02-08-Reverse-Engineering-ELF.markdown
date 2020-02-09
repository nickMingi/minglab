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

1. greadelf -a : dumps everything about the file
2. greadelf -h : dump the ELF header fields
3. greadelf --sections --wide : dump the sections
4. greadelf --segments --wide : dump the segments
5. greadelf -x : dump contents of section

# Tools
1. ghidra
    - download ghidra and add environment variables
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