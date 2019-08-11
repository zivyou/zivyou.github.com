---
title: gdbNotes
date: 2018-11-01 17:26:28
tags:
---

- start:
- https://stackoverflow.com/questions/11408041/how-to-debug-the-linux-kernel-with-gdb-and-qemu
    qemu-system-i386  -s -monitor stdio -fda floppy.img -boot a
    -s for a gdbserver

- ensure that lo eth are up
- gdb toyos_kernel
    - input "target remote localhost:1234" in gdb command line
    - port 1234 is the default debug port for qemu
    - monitor exit: to exit a connection
- check register in gdb:
    - info all-register: list all registers 
    - x/4ubh $eax
        - x for examine, 4ub for 4 unsigned bytes, h for hexadecimal. $eax is the register
- check stack in gdb:
    - x/10x $sp
        - print front 10*4 bytes of the stack
