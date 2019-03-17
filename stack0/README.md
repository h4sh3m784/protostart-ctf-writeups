## This is the writeup for how to solve Protostar-stack0 challange..

The code:

    #include <stdlib.h>
    #include <unistd.h>
    #include <stdio.h>

    int main(int argc, char **argv)
    {
        volatile int modified;
        char buffer[64];

        modified = 0;
        gets(buffer);

        if(modified != 0) {
            printf("you have changed the 'modified' variable\n");
        } else {
            printf("Try again?\n");
        }
    }

#### Objective
modify the "modified" variable, so that we can change the flow of the if/else statement.

#### How?
We can use the "buffer" array to create a buffer overflow attack.


#### Let's take a closer look at it in Assembly.

Compile:

    $ gcc -m32 -fno-stack-protector stack0.c

GDB:

    $ gdb -q a.exe

when we type in to following command:

    $ gdb disas main

we get the main() function:

    0x00401460 <+0>:     push   ebp                      
    0x00401461 <+1>:     mov    ebp,esp                  
    0x00401463 <+3>:     and    esp,0xfffffff0           
    0x00401466 <+6>:     sub    esp,0x60                 
    0x00401469 <+9>:     call   0x4019e0 <__main>        
    0x0040146e <+14>:    mov    DWORD PTR [esp+0x5c],0x0 
    0x00401476 <+22>:    lea    eax,[esp+0x1c]           
    0x0040147a <+26>:    mov    DWORD PTR [esp],eax      
    0x0040147d <+29>:    call   0x403ab0 <gets>          
    0x00401482 <+34>:    mov    eax,DWORD PTR [esp+0x5c] 
    0x00401486 <+38>:    test   eax,eax                  
    0x00401488 <+40>:    setne  al                       
    0x0040148b <+43>:    test   al,al                    
    0x0040148d <+45>:    je     0x40149d <main+61>       
    0x0040148f <+47>:    mov    DWORD PTR [esp],0x405064 
    0x00401496 <+54>:    call   0x403a90 <puts>          
    0x0040149b <+59>:    jmp    0x4014a9 <main+73>       
    0x0040149d <+61>:    mov    DWORD PTR [esp],0x40508d 
    0x004014a4 <+68>:    call   0x403a90 <puts>          
    0x004014a9 <+73>:    mov    eax,0x0                  
    0x004014ae <+78>:    leave                           
    0x004014af <+79>:    ret                             