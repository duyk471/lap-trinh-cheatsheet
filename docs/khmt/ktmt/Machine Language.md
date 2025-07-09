### Rất chi là cơ bản
Ngôn ngữ máy là cách mà các lập trình viên *trò chuyện* với máy tính và để đưa ra các *instructions* cho nó. Có hai kiểu ngôn ngữ máy là: binary code (Kiểu viết thẳng là 00101000101) với symbolic code (Khởi nguyên là assembly, kiểu như `ADD R3 R2 R1`, nghĩa là, cộng R1 với R2 rồi lưu giá trị vào R3). One can specify machine language instructions either directly, as 1010001100011001, or symbolically, as, say, ADD R3,R1,R9.

Để hiểu được instruction: The instruction set of the underlying hardware platform. For example, the language may besuch that each instruction consists of four 4-bit ﬁelds: The left-most field codes a CPU operation, and the remaining three ﬁelds represent the operation’s operands. Thus the previous command may code the operation set R3 to R1 + R9, depending of course on the hardware specification and the machine language syntax

The symbolic notation is called assembly language, or simply [[Assembly]], and the program that translates from assembly to binary is called assembler.
### Arithmetic and Logic Operations

```
ADD R2,R1,R3 // R2<---R1+R3 where R1,R2,R3 are registers
ADD R2,R1,foo 
// R2<---R1+foo where foo stands for the
// value of the memory location pointed at by the user-defined label foo.
AND R1,R1,R2 // ---bit wise And of R1 and R2
```
### Memory Access
**Direct addressing**: The most common way to address the memory is to express a speciﬁc address or use a symbol that refers to a speciﬁc address, as follows:
```
LOAD R1,67 // R1 <--- Memory[67]
// Or, assuming that bar refers to memory address 67:
LOAD R1,bar // R1 <--- Memory[67]
```
**Immediate addressing:** This form of addressing is used to load constants— namely, load values that appear in the instruction code: Instead of treating the numeric ﬁeld that appears in the instruction as an address, we simply load the value of the ﬁeld itself into the register, as follows:
```
LOADI R1,67 // R1---67
```
**Indirect addressing**: In this addressing mode the address of the required memory location is not hard-coded into the instruction; instead, the instruction speciﬁes a memory location that holds the required address. This addressing mode is used to handle pointers (Như kiểu trong C ấy, đặt một biến với kiểu dữ liệu có dấu * để biểu thị nó là một pointer).
```
// Translation of x=foo[j] or x=*(foo+j):
ADD R1,foo,j // R1<---foo+j
LOAD* R2,R1 // R2<---Memory[R1]
STR R2,x // x<---R2
```

### Flow of control
While programs normally execute in a linear fashion, one command after the other, they also include occasional branches to locations other than the next command.

Branching serves several purposes including:
- Repetition ( jump backward to the beginning of a loop)
- Conditional execution (if a Boolean condition is false, jump forward to the location after the ‘‘if-then’’ clause)
- Subroutine calling (jump to the ﬁrst command of some other code segment).

Ví dụ cho phần code ở ngôn ngữ High-level
```
// A while loop:
while (R1>=0) {
	code segment 1
}
code segment 2
```

Low-level
```
// Typical translation:
beginWhile:
	JNG R1,endWhile // If R1<0 goto endWhile
	// Translation of code segment 1 comes here
	JMP beginWhile // Goto beginWhile
endWhile:
// Translation of code segment 2 comes here
```
