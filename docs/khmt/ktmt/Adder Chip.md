Adder: a chip designed to add two integers
![[img/Pasted image 20250527090759.png]]
- Half adder: designed to add 2 bits
- Full adder: designed to add 3 bits (Về cơ bản là Half Adder nhưng có lưu thêm overflow bit khi thực hiện tính toán: [[Arithmetic Logic]])
- Adder: designed to add two n-bit numbers.
Một số khái niệm khác cần nhớ (Cái này dùng trong implementation thôi...):
- sum = LSB of a + b
- carry = MSB of a + b
### Half adder
![[img/Pasted image 20250527091707.png]]
Chi để thêm hai số, mà không tính phần dư (carry)

### Full adder
![[img/Pasted image 20250527091741.png]]
Về cơ bản là half adder, nhưng c sẽ chứa cả carry bit từ phép toán half adder trước nữa!
### Adder
![[img/Pasted image 20250527091833.png]]
Implementation: array of full-adder gates.

Từ các chip này chúng ta có thể xây dựng một [[Arithmetic Logic Unit]] hoàn chỉnh, chắc vậy.