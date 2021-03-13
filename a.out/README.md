## Write up on the a.out challenge on Crackmes (https://crackmes.one/crackme/5f05ec3c33c5d42a7c66792b)

Hi, this is my absolute first crack and I'm really happy that I managed to crack it successfully.

This is how I did it:

1. I opened the file in gdb

1.2. I ran "set disassembly-flavor intel" since that makes it more comfortable to read, in my opinion. It's not necessary to solve the challenge.

2. Since all C programs have a main function I ran:

    disassemble main
    
    Then you notice that the main function calls for the "login" function: PICTURE
    
3. Since I found that there was another function being called I disassebled that one too doing:

    disassemble login
    
    We notice that there's alot more output this time.
    I read through the output and focues mainly on "call", "test", "jne" and "jmp" since they control the flow of the program.
    
    On the first "call" we see that it calls the function "printf" which prints text to the screen, however when we run it
    we don't notice any output but it continues to run without problem. 
    
4. If we take a look at the call to "fgets" we know that, that function takes user input so we set a break point just before it to make sure.

    break *0x00005555555551bb
    
    I move forward using "ni" and we confirm that we get asked to insert input. 
    
5. The next call is for "printf" and this time it actually returns a value. We set a breakpoint there aswell to make sure.

    And we are correct it returns a string: uid: 1
    
6. Now we are at the test eax, eax which evaluates these values and returns 0 if they are the same.

    We look at the contents of the registers by doing:
    
    info registers
    
    And get back that eax contains 1 since the password we passed wasn't the same as the one being tested. We want the "jne", the next call, to be
    false since we don't want to jump. Therefore we need to change the value in the eax register to 0 to indicate that the evaluation was correct:
    
    set $eax=0
    
    If we run info registers again we can see that rax (meaning the whole 64 bit value) is equal to 1, perfect!
    
7. We now continue the process by doing "ni" as before.

    And we notice that we didn't jump, perfect!
    
    We continue going through and then when we meet the call to "puts" we get the message "you are logged in as admin" printed in the console.
    
    
    
    
    
    
    
    
    
