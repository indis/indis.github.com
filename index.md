---
layout: page
title: Indis
tagline: The intelligent disassembly framework
---
{% include JB/setup %}

Indis is an intelligent disassembly framework. The primary goal for indis is to provide facilities for analysis of binary files. The plan is to provide a highly extensible object model for binary contents with support of reversing back up to C-like code.

Indis consists of a [core](http://github.com/indis/indis-core) gem, that unifies all the different target formats and architectures (which is a pretty hard task for now, when I'm focused on Mach-O ARM targets only); format processing gems (like [Mach-O](http://github.com/indis/indis-macho)), that provide support of loading the specific binary format and collecting generic info about it (stuff like virtual map of sections, symbol references, etc.); and cpu processing gems ([ARM](http://github.com/indis/indis-arm) for now).

### Source Code

You can find the gems source at [GitHub](http://github.com/indis).

### API Documentation

 - [indis-core](http://rdoc.info/github/indis/indis-core/frames)
 - [indis-macho](http://rdoc.info/github/indis/indis-macho/frames)
 - [indis-arm](http://rdoc.info/github/indis/indis-arm/frames)

### Updates

See the [archive](/archive.html) for updates on the project.

### Show me some output!

Why, here you go ;)

    % bin/indis-dis app-arm-release.o _main
    000021b4 e92d40f0	PUSH	{r4, r5, r6, r7, lr}	
    000021b8 e28d700c	ADD	r7, sp, #12	
    000021bc e1a05001	MOV	r5, r1	
    000021c0 e1a06000	MOV	r6, r0	
    000021c4 eb000388	BL	0x2fec	; branch_to_sym: _objc_autoreleasePoolPush
    000021c8 e3011324	MOVW	r1, #4900	
    000021cc e3401000	MOVT	r1, #0	
    000021d0 e1a04000	MOV	r4, r0	
    000021d4 e79f1001	LDR	r1, [pc, r1]	
    000021d8 e3010360	MOVW	r0, #4960	
    000021dc e3400000	MOVT	r0, #0	
    000021e0 e79f0000	LDR	r0, [pc, r0]	
    000021e4 eb000381	BL	0x2ff0	; branch_to_sym: _objc_msgSend
    000021e8 eb00037c	BL	0x2fe0	; branch_to_sym: _NSStringFromClass
    000021ec e1a01005	MOV	r1, r5	
    000021f0 e3a02000	MOV	r2, #0	
    000021f4 e1a03000	MOV	r3, r0	
    000021f8 e1a00006	MOV	r0, r6	
    000021fc eb000375	BL	0x2fd8	; branch_to_sym: _UIApplicationMain
    00002200 e1a05000	MOV	r5, r0	
    00002204 e1a00004	MOV	r0, r4	
    00002208 eb000376	BL	0x2fe8	; branch_to_sym: _objc_autoreleasePoolPop
    0000220c e1a00005	MOV	r0, r5	
    00002210 e8bd80f0	POP	{r4, r5, r6, r7, pc}

### Build Status

#### Integration Tests (indis meta gem)
[![Build Status](https://secure.travis-ci.org/indis/indis.png?branch=master)](http://travis-ci.org/indis/indis)

#### Core
[![Build Status](https://secure.travis-ci.org/indis/indis-core.png?branch=master)](http://travis-ci.org/indis/indis-core)

#### Mach-O
[![Build Status](https://secure.travis-ci.org/indis/indis-macho.png?branch=master)](http://travis-ci.org/indis/indis-macho)

#### ARM
[![Build Status](https://secure.travis-ci.org/indis/indis-arm.png?branch=master)](http://travis-ci.org/indis/indis-arm)

### License

This project is distributed under the terms of [GPL-3](http://www.gnu.org/licenses/gpl.html) license.