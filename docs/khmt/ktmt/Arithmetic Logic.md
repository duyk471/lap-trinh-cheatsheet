Assuming a 4-bit system:
![[img/Pasted image 20250527090914.png]]
Algorithm: exactly the same as in decimal addition. Overflow (MSB carry) has to be dealt with.
Có hai loại "khoảng dữ liệu" (?) là signed và unsigned (Ví dụ về range thì với 8-bit chẳng hạn, thì với unsigned ta có thể sử dụng các số từ 0 cho tới 255, nhưng với signed thì là -127 cho tới +127).

Để thể hiệu dấu âm trong số (signed) thì ta sử dụng Two's Complement
![[img/Pasted image 20250527091526.png]]
Một số các quy tắc:
- The codes of all positive numbers begin with a “0”
- The codes of all negative numbers begin with a “1“
- To convert a number: leave all trailing 0’s 1 and first 1 intact, and flip all the remaining bits
![[img/Pasted image 20250527091553.png]]

Quy tắc về cơ bản chỉ có vậy thôi, ở đây chưa implement nhân chia.
