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
