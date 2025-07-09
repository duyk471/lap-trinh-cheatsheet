**Định nghĩa:** Là một thiết bị để implement boolean function Boolean function bao gồm (Được gọi là cổng logic với implementation cho hàm f với n input pins và m output pins):
- Hàm f
- n biến
- m binary results
Và giống như các hàm Boolean phức tạp có thể được biểu diễn sử dụng các hàm cơ bản, các cổng phức tạp được tạo nên từ các cổng cơ bản. Các gates đơn giản nhất được làm từ thiết bị bật tắt (switching devices) siêu đơn giản có tên là transistors, được nối các kiểu con đà điểu để thành một cái cổng phức tạp hơn.

![[img/Pasted image 20250526204839.png]]
#### Primitive and Composite Gates

Hai kiểu cổng là primitive gate (Kiểu and, or, nand) và composite gate (Các kiểu cổng phức tạp hơn, kết hợp giữa nhiều cổng primitive với nhau). Vì tất cả các cổng logic đều có cùng input and output semantics (0s và 1s), chúng có thể được nối với nhau, tạo ra composite gates có độ phức tạp khác nhau.
![[img/Pasted image 20250526205037.png]]
Đây là một ví dụ đơn giản về gate logic, còn được gọi là logic design.

Chúng ta thấy rằng bất kỳ cổng logic nào đã cho đều có thể được xem từ hai khía cạnh khác nhau: external và internal. Phía bên phải của hình đưa ra internal architecture, hay implementation, của gate, trong khi phía bên trái chỉ hiển thị gate interface, cụ thể là các input and output pins mà nó tương tác. Phần trước thì phù hợp cho những người _sử dụng_, chỉ quan tâm về cái _what_ hay _nó làm cái gì_. Còn phần sau thì dành cho các chế thiết kế phần cứng để hiểu về cách nó vận hành, cách mà nó _được implemented_.

Tóm lại, nghệ thuật của logic design có thể được mô tả thế này: *Cho một gate specification (interface), tìm một cách hiệu quả để implement nó sử dụng các gates khác đã được implemented trước đó*.

#### Cổng Mux & Demux

Multiplexor A multiplexor is a three-input gate that uses one of the inputs, called "selection bit" to select and output one of the other two inputs, called "data bits"
![[img/Pasted image 20250526205010.png]]