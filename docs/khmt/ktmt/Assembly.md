### Ngôn ngữ (Assembly)
Thay vì viết trực tiếp ngôn ngữ máy (Machine Language) để giao tiếp với ngôn ngữ, ta chuyển qua sử dụng *symbolic code*. Sẽ hay hơn khi viết dựa trên một số cú pháp được quy định sẵn kiểu LOAD R3,7 rather than 110000101000000110 000000000000111. The symbolic language is called **assembly**, and the translator program **assembler**.
Cần phân biệt giữa variables và labels:
- Variables để lưu trữ giá trị
- Labels: one can declare the label loop to refer to the beginning of a certain code segment (Để đánh dấu chức năng của các code segment nhất định).
Phân vùng kiểu dữ liệu & không phải mỗi instruction đều == 1 word, và tương tự, không phải bao giờ data type cũng chỉ chiếm đúng một word width. Giả sử $w=16bit$ đi, thì một biến double (~64bit) sẽ chiếm 4 $words$, kiểu vậy.
### Trình biên dịch (Assembler)
Mỗi họ vi xử lý có một bộ chỉ dẫn(**instructions**) riêng cho các hoạt động xử lý khác như nhập vào bàn phím, xuất thông tin ra màn hình và nhiều công việc khác. Những thiết lập chỉ dẫn này được gọi là '**machine language instructions** (Không nhất thiết phải theo đúng thứ tự):
- Parse the symbolic command into its underlying ﬁelds.
- For each ﬁeld, generate the corresponding bits in the machine language.
- Replace all symbolic references (if any) with numeric addresses of memory locations.
- Assemble the binary codes into a complete machine instruction.

(Sẽ học viết Assembly sau.... chỉ là không biết là bao giờ thôi)