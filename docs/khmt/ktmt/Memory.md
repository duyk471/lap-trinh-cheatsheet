### Khái niệm cơ bản về Memory
Memory: The term memory refers loosely to the collection of hardware devices that store data and instructions in a computer. All memories have the same structure: A continuous array of cells of some fixed width, also called words or locations, each having a unique address.
### RAM
A direct-access memory unit, also called RAM, is an array of n w-[[Bit]] registers, equipped with direct access circuitry. The number of registers (n) and the width of each register (w) are called the memory’s **size** and **width**, respectively. Stacking together many [[Register]] to form a Random Access Memory (RAM) unit. Implement cái RAM (Về cơ bản là một Stack chứa nhiều Registers) với một cơ chế đặc biệt khác để tra cứu dữ liệu dựa trên địa chỉ
Modern computers typically employ 32- or 64-bit-wide RAMs whose sizes are up to hundreds of millions.
![[img/Pasted image 20250526205528.png]]

> a classical RAM device accepts three inputs: a data input (*word*), an address in-put, and a load bit.
> 
> The address speciﬁes which RAM register should be accessed in the current time unit. In the case of a read operation (load=0), the RAM’s outputimmediately emits the value of the selected register.
> 
> In the case of a write operation (load=1), the selected memory register commits to the input value in the next time unit, at which point the RAM’s output will start emitting it.

![[img/Pasted image 20250526205533.png]]
The basic design parameters of a RAM device are:
- **Data width của RAM** thường là kích thước của một word.
- Size — the number of words in the RAM. 

### ROM
ROM, refer to Read-Only Memory which contains fixed instructions and retains information without power



