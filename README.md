# PanicConverter
Converts a OSX Kernel Panic into a human readable stack trace

This program converts OSX kernel panics into something more human readable.

To use it, you should have installed atos, which comes with XCode.

The usage is simple:
After a kernel panic happened, you receive a system prompt reporting something like this

```
Fri Jul  1 23:06:47 2016

*** Panic Report ***
panic(cpu 6 caller 0xffffff8006dce40a): Kernel trap at 0xffffff7f89d2e3d1, type 13=general protection, registers:
CR0: 0x000000008001003b, CR2: 0x0000000167461000, CR3: 0x000000000a501000, CR4: 0x00000000001627e0
RAX: 0xffffff7f87fca0a0, RBX: 0xffffff7f89d2d3d0, RCX: 0x0000000000000000, RDX: 0xffffff8026611600
RSP: 0xffffff81f1923b50, RBP: 0xffffff81f1923b60, RSI: 0x0000000000000020, RDI: 0xffffff802d537400
R8:  0x000000000000ffff, R9:  0x0000000000000001, R10: 0xffffff8026a8dc00, R11: 0x0000000000000000
R12: 0x0000000000000000, R13: 0x00000000fffffffd, R14: 0xffffff8036045ea0, R15: 0xffffff8036045d10
RFL: 0x0000000000010286, RIP: 0xffffff7f89d2e3d1, CS:  0x0000000000000008, SS:  0x0000000000000010
Fault CR2: 0x0000000167461000, Error code: 0x0000000000000000, Fault CPU: 0x6, PL: 0

Backtrace (CPU 6), Frame : Return Address
0xffffff81e6f9ddf0 : 0xffffff8006cdab12 
0xffffff81e6f9de70 : 0xffffff8006dce40a 
0xffffff81e6f9e050 : 0xffffff8006dec273 
0xffffff81e6f9e070 : 0xffffff7f89d2e3d1 
0xffffff81f1923b60 : 0xffffff7f89d2e237 
0xffffff81f1923bb0 : 0xffffff7f89d2d4da 
0xffffff81f1923e00 : 0xffffff7f89d2d423 
0xffffff81f1923e50 : 0xffffff80072b5958 
0xffffff81f1923ec0 : 0xffffff7f89d2d30d 
0xffffff81f1923f00 : 0xffffff8006d0f1ea 
0xffffff81f1923fb0 : 0xffffff8006dc8e27 
      Kernel Extensions in backtrace:
         com.emu.driver.EMUUSBAudio(3.5.5)[5943304A-03E2-3343-92AD-24255DBB8635]@0xffffff7f89d1c000->0xffffff7f89d7ffff
            dependency: com.apple.iokit.IOAudioFamily(204.3)[79080C52-FC35-31BA-8C06-087B308D33D1]@0xffffff7f89150000
            dependency: com.apple.iokit.IOUSBHostFamily(1.0.1)[4C8B5BB6-6AE4-313E-B79C-AC07A4E31A2D]@0xffffff7f87eaf000

```

You copy it to a file, say "panic.txt" and then you can start the convertpanic program like this

``` awk -f convertpanic panic.txt```

It will show you the actual stacktrace for this panic (and some un-suppressable atos blurps):

```
TRACE:
0xffffff8006cdab12
0xffffff8006dce40a
0xffffff8006dec273
IOUSBInterface1::getDevice1() (in EMUUSBAudio) (IOUSBInterface.h:117)
EMUUSBAudioDevice::checkUHCI() (in EMUUSBAudio) (EMUUSBAudioDevice.cpp:355)
EMUUSBAudioDevice::protectedInitHardware(IOService*) (in EMUUSBAudio) (EMUUSBAudioDevice.cpp:192)
EMUUSBAudioDevice::initHardwareThreadAction(OSObject*, void*, void*, void*, void*) (in EMUUSBAudio) (EMUUSBAudioDevice.cpp:177)
0xffffff80072b5958
EMUUSBAudioDevice::initHardwareThread(EMUUSBAudioDevice*, void*) (in EMUUSBAudio) (EMUUSBAudioDevice.cpp:168)
0xffffff8006d0f1ea
0xffffff8006dc8e27
```
