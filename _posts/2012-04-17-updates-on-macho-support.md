---
layout: post
title: "Updates on Mach-O support"
category: Mach-O
tags: [mach-o, core, indis-seg]
---
{% include JB/setup %}

I've rolled out two updates for Mach-O support while trying to track down, how does otool resolve the symbol names in stub section.

Somehow I've got distracted on to __LC_DYLD_INFO__ load command, and spent some time figuring how I should parse the bytecode. There's an awesome reference [here](http://networkpx.blogspot.com/2009/09/about-lcdyldinfoonly-command.html), but I've found it more helpful to dig through [dyld sources](http://opensource.apple.com/source/dyld/dyld-132.13/src/ImageLoaderMachOCompressed.cpp). Actually the biggest issue was that the format uses integer overflow to do some arithmetic, which I figured after tracking parts of dyld inside of lldb.

So, now indis-macho can parse most of the __LC_DYLD_INFO__ (excluding relocation end external parts). Still, what I tried to achieve was "hidden" inside of __LC_DYSYMTAB__, namely the indirect symbols reference. I'm not yet sure if it's possible to look up all the references somehow, so, for now, I'm going to find some way of hooking into instructions creation process and resolve such symbols on BL'ing to them (much like otool does, actually).

Now, the second update is for both macho and core. The sections now have a type and attributes (passed over from macho header for now). That would really simplify per-section analysis. And a neat update to indis-seg:

{% highlight bash %}
% bundle exec bin/indis-seg app-arm-release.o
        segname          sectname    vmaddr    vmsize   fileoff  filesize  type
     __PAGEZERO                    00000000      4096  00000000         0
         __TEXT                    00001000      8192  00000000      8192
                           __text  00002188      1104  00001188            S_REGULAR 
                    __stub_helper  000025D8       156  000015D8            S_REGULAR 
                  __objc_methname  00002674      1371  00001674            S_CSTRING_LITERALS 
                        __cstring  00002BCF       148  00001BCF            S_CSTRING_LITERALS 
                 __objc_classname  00002C63        62  00001C63            S_CSTRING_LITERALS 
                  __objc_methtype  00002CA1       821  00001CA1            S_CSTRING_LITERALS 
                    __symbolstub1  00002FD8        40  00001FD8            S_SYMBOL_STUBS 
         __DATA                    00003000      4096  00002000      4096
                    __lazy_symbol  00003000        40  00002000            S_LAZY_SYMBOL_POINTERS 
                   __program_vars  00003030        20  00002030            S_REGULAR 
                  __nl_symbol_ptr  00003044         8  00002044            S_NON_LAZY_SYMBOL_POINTERS 
                 __objc_classlist  0000304C         8  0000204C            S_REGULAR 
                 __objc_protolist  00003054         8  00002054            S_REGULAR 
                 __objc_imageinfo  0000305C         8  0000205C            S_REGULAR 
                     __objc_const  00003068      1176  00002068            S_REGULAR 
                   __objc_selrefs  00003500        72  00002500            S_LITERAL_POINTERS 
                 __objc_classrefs  00003548        16  00002548            S_REGULAR 
                 __objc_superrefs  00003558         8  00002558            S_REGULAR 
                      __objc_data  00003560        80  00002560            S_REGULAR 
                      __objc_ivar  000035B0         8  000025B0            S_REGULAR 
                       __cfstring  000035B8        48  000025B8            S_REGULAR 
                           __data  000035E8        88  000025E8            S_REGULAR 
                         __common  00003640        16  00000000            S_ZEROFILL 
     __LINKEDIT                    00004000     12288  00003000     11568
{% endhighlight %}