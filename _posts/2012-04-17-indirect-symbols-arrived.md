---
layout: post
title: "Indirect symbols arrived!"
category: Mach-O
tags: [mach-o, core]
---
{% include JB/setup %}

Mach-O loader now behaves much better on what symbols are passed up to Target. Alongside with this, I've added indirect symbols resolution via __LC_DYSYMTAB__. Here you can see it in action:

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

Next stop -- building a function graph, stay tuned!