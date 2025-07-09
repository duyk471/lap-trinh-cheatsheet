Combinational chips: operate on data only; provide calculation services (e.g. Nand … ALU) -> Để xây dựng những chức năng xử lý quan trọng như là ALU.  Nhưng chúng không thể duy trì state (store and recall values). Vậy nên, cần có memory để lưu trữ dữ liệu theo thời gian. These memory elements are built from sequential chips.
### Cái đồng hồ
In most computers, the passage of time is represented by a master clock that delivers a continuous train of alternating signals (alternates continuously between two phases labeled 0–1, low-high, tick-tock, etc) The elapsed time between the beginning of a ‘‘tick’’ and the end of the subsequent ‘‘tock’’ is called **cycle**.
The current clock phase (tick or tock) is *represented by a binary signal*.
> Mỗi một cycle thường rơi vào khoảng 1/2 nanosecond
![[img/Pasted image 20250527084240.png]]
### Flip-flop
Về cơ bản thì flip-flop là "A fundamental state-keeping device". Trong Nand2Tetris thì sẽ sử dụng: data flip-flop (DFF)
![[img/Pasted image 20250527084143.png]]
$out(t) = in(t - 1)$
`in` and `out` are the gate’s input and output values and t is the current clock cycle => DFF outputs the input value from the previous time unit.
