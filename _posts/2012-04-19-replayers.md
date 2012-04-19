---
layout: post
title: "Replayers"
category: arm
tags: [reversing]
---
{% include JB/setup %}

I had a few ideas on how a function should be reversed. It didn't came quite well to split the code into indis-core and indis-arm, so, for now, the features are provided by indis-arm gem directly. That might change in future, when we have some support for other architectures.

So, what do we have at this point?

    8628 PUSH {r4, r5, r6, r7, lr} 
    8632 ADD  r7, sp, #12
    8636 MOV  r5, r1               ; arg2
    8640 MOV  r6, r0               ; arg1
    8644 BL   0x2fec               ; _objc_autoreleasePoolPush(arg1, arg2, arg3, arg4)
    8648 MOVW r1, #4900            ; 4900
    8652 MOVT r1, #0               ; 4900
    8656 MOV  r4, r0               ; 8644_ret1
    8660 LDR  r1, [pc, r1]         ; load from 13568
    8664 MOVW r0, #4960            ; 4960
    8668 MOVT r0, #0               ; 4960
    8672 LDR  r0, [pc, r0]         ; load from 13640
    8676 BL   0x2ff0               ; _objc_msgSend(_OBJC_CLASS_$_DAAppDelegate, "class")
    8680 BL   0x2fe0               ; _NSStringFromClass(8676_ret1)
    8684 MOV  r1, r5               ; arg2
    8688 MOV  r2, #0               ; 0
    8692 MOV  r3, r0               ; 8680_ret1
    8696 MOV  r0, r6               ; arg1
    8700 BL   0x2fd8               ; _UIApplicationMain(arg1, arg2, 0, 8680_ret1)
    8704 MOV  r5, r0               ; 8700_ret1
    8708 MOV  r0, r4               ; 8644_ret1
    8712 BL   0x2fe8               ; _objc_autoreleasePoolPop(8644_ret1)
    8716 MOV  r0, r5               ; 8700_ret1
    8720 POP  {r4, r5, r6, r7, pc}

The metadata is provided by just a few replayers, namely for BL, LDR, MOV instructions. There's also an register modification replayer, but it's more like a monitor. This one is used to track modifications to PC, that trigger the returning point of function.

I don't really like how it all looks in the code, so I will be experimenting with it for a few days. The changed are not merged into master, but they are available in the [features/function_builder](https://github.com/indis/indis-arm/tree/features/function_builder) branch.