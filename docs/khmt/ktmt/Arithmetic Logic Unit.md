Là một thành phần trong [[CPU]], thực hiện các nhiệm vụ liên quan đến tính toán ([[Arithmetic Logic]]). Được xây dựng từ các [[Adder Chip]]. The (Hack) ALU computes a ﬁxed set of functions $out=fi(x, y)$ where x and y are the chip’s two 16-bit inputs, out is the chip’s 16-bit output, and fi is an arithmetic or logical function selected from a ﬁxed repertoire of eighteen possible functions (Ở đây ngữ cảnh là Hack ALU, còn lại thì tương tự).
![[img/Pasted image 20250527091933.png]]
6 cái mũi tên ở hướng trên tượng trưng cho 6 control bits (We instruct the ALU which function to compute by setting six input bits, called control bits, to selected binary values). Dưới đây là bảng cho 6 control bits ở trên
![[img/Pasted image 20250527092413.png]]
Còn một phần nữa là cái Output (Không nhớ chính xác là nó để làm gì):
```
zr // True iff out=0
ng // True iff out<0
```

