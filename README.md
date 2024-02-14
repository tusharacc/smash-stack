### Stack

    +-------------------------------------+
    + Arguments to Function               +
    +-------------------------------------+
    + Return Address                      +
    +-------------------------------------+
    + Return Address (Prev FP)            +
    +-------------------------------------+
    + Current Frame Pointer               +
    +-------------------------------------+
    + Local Variables                     +
    +-------------------------------------+ <------ Stack Pointer

These are the steps to form stack during function call.

+ The arguments are added to the stack
+ Return address (the address after call statement) is pushed on stack
+ Current Frame Pointer is saved 
+ Current Stack Pointer is saved in Frame Pointer
+ Area is allocated for the stack

#### Understanding Stack using a program

`
void function(int a, int b, int c){
    char buffer1[5];
    char buffer2[10];
}

void main(){
    function(1,2,3);
}
`

As mentioned in the document, `gcc -S -o example1.s example1.c`

> -S will generate the assembler code.

| Assembler Code                | In English                                                      |
|-------------------------------|-----------------------------------------------------------------|
|pushq	%rbp                    | saving base/frame pointer                                       |
|movq	%rsp, %rbp              | moving stack pointer & saving as current function frame pointer |
|subq	$48, %rsp               | Subtracting x30(48) from Stack pointer.*                        |
|movl	%edi, -36(%rbp)         | Moving arguments to register*                                   |
|movl	%esi, -40(%rbp)         | Moving arguments to register                                    | 
|movl	%edx, -44(%rbp)         | Moving arguments to register                                    |
|movq	%fs:40, %rax            |                                                                 |
|movq	%rax, -8(%rbp)          |                                                                 |
|movq	-8(%rbp), %rax          |                                                                 |
|subq	%fs:40, %rax            |                                                                 |

*Why Subract - because Stack grows from high address to low address
*In GCP, it subtracted 10