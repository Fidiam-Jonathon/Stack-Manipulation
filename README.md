# Stack-Manipulation

Completion time: 2 hours

Purpose: demonstrate mastery of the run-time stack by chaning values in a function without passing them in as parameters.

# How did I do it?
the assignment essentially wanted me to guess where the variables where and change their values through trial and error.  I had been studying
assembly dumps and object files on the side and realized I could use this knowledge to complete the task much faster.  I compiled the program,
loaded up GDB and ran a disass on the two functions I was interested in.  This is the assembly dump from readStack()

   0x00401a9f <+0>:     push   %ebp
   0x00401aa0 <+1>:     mov    %esp,%ebp
   0x00401aa2 <+3>:     push   %ebx
   0x00401aa3 <+4>:     sub    $0x44,%esp
   0x00401aa6 <+7>:     movl   $0x5,-0x1c(%ebp)
   0x00401aad <+14>:    movb   $0x42,-0x2c(%ebp)
   0x00401ab1 <+18>:    lea    -0x2c(%ebp),%eax
   0x00401ab4 <+21>:    add    $0x60,%eax
   0x00401ab7 <+24>:    mov    %eax,-0x10(%ebp)
   0x00401aba <+27>:    lea    -0x2c(%ebp),%eax
   0x00401abd <+30>:    add    $0x77,%eax
   0x00401ac0 <+33>:    mov    %eax,-0x14(%ebp)
   0x00401ac3 <+36>:    lea    -0x1c(%ebp),%eax
   0x00401ac6 <+39>:    add    $0x54,%eax
   0x00401ac9 <+42>:    mov    %eax,-0x18(%ebp)
   0x00401acc <+45>:    lea    -0x30(%ebp),%eax
   0x00401acf <+48>:    add    $0x40,%eax
   0x00401ad2 <+51>:    movl   $0x401c15,(%eax)
   0x00401ad8 <+57>:    mov    -0x18(%ebp),%eax
   0x00401adb <+60>:    mov    (%eax),%eax
   0x00401add <+62>:    mov    %eax,-0x1c(%ebp)
   0x00401ae0 <+65>:    mov    -0x14(%ebp),%eax
   0x00401ae3 <+68>:    movb   $0x41,(%eax)
   0x00401ae6 <+71>:    movl   $0x0,-0xc(%ebp)
   0x00401aed <+78>:    cmpl   $0xf,-0xc(%ebp)
   0x00401af1 <+82>:    jg     0x401b0e <_Z9readStackv+111>
   0x00401af3 <+84>:    mov    -0xc(%ebp),%edx
   0x00401af6 <+87>:    mov    -0x10(%ebp),%eax
   0x00401af9 <+90>:    add    %edx,%eax
   0x00401afb <+92>:    movzbl (%eax),%eax
   0x00401afe <+95>:    lea    -0x2c(%ebp),%ecx
   0x00401b01 <+98>:    mov    -0xc(%ebp),%edx
   0x00401b04 <+101>:   add    %ecx,%edx
   0x00401b06 <+103>:   mov    %al,(%edx)
   0x00401b08 <+105>:   addl   $0x1,-0xc(%ebp)
   0x00401b0c <+109>:   jmp    0x401aed <_Z9readStackv+78>
   0x00401b0e <+111>:   mov    -0x1c(%ebp),%ebx
   0x00401b11 <+114>:   movl   $0x406175,0x4(%esp)
   0x00401b19 <+122>:   movl   $0x4092ec,(%esp)
   
   the first six instructions are what I needed to know. The first 2 are what is known as the function prolog, this is how the stack
   is managed.  The first instruction moves the base pointer (ebp, the e is for extended, which means it is 32-bit) onto the stack, and
   the second instruction copies the current value of the stack pointer (SP) into the base pointer (BP).  Instruction 4, subtracting the SP
   is making room for the local variables.  
   
   Since there are no symbolic identifiers (i.e. names) of variables in this dissassembly dump, I used the trick of placing known values in
   the variables, that's what lines 5 and 6 do.  I used their relative offsets to see how far into the function they were.  The next step
   was to do the same thing on partThree().
   
   One final note: the offsets that I calculated are platform specific, this program will only work correctly on a Linux distro running GCC
