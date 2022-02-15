rev/wstrings
Author: NotDeGhost

Some strings are wider than normal...

## Solution
The description and the title of the challenge gives us a good idea of where to start.

We first start by running strings on the file, but we get nothing so lets open the file with ghidra.

Looking at the code of the main function, nothing seems useful to us.

```c
undefined8 main(void) {
  int iVar1;
  long in_FS_OFFSET;
  wchar_t local_158 [82];
  long local_10;
  
  local_10 = *(long *)(in_FS_OFFSET + 0x28);
  wprintf(L"Welcome to flag checker 1.0.\nGive me a flag> ");
  fgetws(local_158,0x50,stdin);
  iVar1 = wcscmp((wchar_t *)flag,local_158);
  if (iVar1 == 0) {
    fputws(L"Correct!",stdout);
  }
  if (local_10 != *(long *)(in_FS_OFFSET + 0x28)) {
                    /* WARNING: Subroutine does not return */
    __stack_chk_fail();
  }
  return 0;
}```

Just looks like it compares with the actual flag.

Lets now check in the comments of the disassembly code.

Looking close to the wcscmp function (string compare), we see a flag!

## Flag
```flag{n0t_al1_str1ngs_ar3_sk1nny}```
