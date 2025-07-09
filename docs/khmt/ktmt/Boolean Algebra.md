### Đại số Boolean
Là môn đại số xoay quanh việc xử lý các giá trị Boolean (hay còn gọi là *giá trị binary*) kiểu true/false, 1/0, yes/no, on/off, v.v. (Bài viết này sẽ sử dụng 1 và 0).
#### Hàm Boolean (boolean function)
Là hàm nhận vào các binary input và trả về các binary output. Chúng ta có thể kể đến một số hàm Boolean đơn giản như:
- Not(x)
- And(x, y)
- Or(x, y)
- Nand(x, y)
Chúng ta có thể sử dụng: functional expression hoặc truth table expression để thể hiện hàm boolean (Sẽ được đề cập bên dưới)
**Important observation:** Every Boolean function can be expressed using And, Or, Not. Hoặc thậm chí, có thể chỉ sử dụng hàm Nand cũng có thể tạo được tất cả.
#### Truth Table Representation
Cách đơn giản nhất để thể hiện Boolean function là liệt kê tất cả các giá trị có thể có của input và output tương ứng.

| $x$ | $y$ | $z$ | $f(x, y, z)$ |
| --- | --- | --- | ------------ |
| 0   | 0   | 0   | 0            |
| 0   | 0   | 1   | 0            |
| 0   | 1   | 0   | 1            |
| 0   | 1   | 1   | 0            |
| 1   | 0   | 0   | 1            |
| 1   | 0   | 1   | 0            |
| 1   | 1   | 0   | 1            |
| 1   | 1   | 1   | 0            |

Ba cột đầu tiên liệt kê tất cả các giá trị binary có thể có của x, y, z trong mỗi function. Chúng ta gọi mỗi một bộ ba ở mỗi hàng trong bảng đó là một tuple (x, y, z), ví dụ như ở hàng 2 thì ta có $(x, y, z) = (0, 0, 1)$ và $f(x, y, z) = 0$ chẳng hạn.
Tại sao lại có 8 hàng? Chúng ta sẽ có $2^n$ tuple với $n$ là số lượng input của hàm boolean, ở đây $n=3$ nên $2^3=8$
#### Boolean Expressions
Sử dụng các Boolean operations để thể hiện hàm boolean. Các Boolean operators cơ bản như:
- $And$: $x$ And $y$ là 1  khi cả $x$ và $y$ đều là 1. Notation: $x.y$ (hoặc $xy$)
- $Or$: $x$ Or $y$ là 1 chính xác khi hoặc $x$ hoặc $y$ hoặc cả hai đều là 1. Notation: $x+y$
- $Not$ Not $x$ là 1 khi $x$ là 0 và ngược lại. Notation: $\bar{x}$

Ta có thể viết lại hàm boolean ở trên thành $f(x, y, z) = (x+y).\bar{z}$.
#### Canonical Representation
Chúng ta có thể biểu diễn mọi Boolean function bằng ít nhất một Boolean expression được gọi là canonical representation. Từ truth table của function, chúng ta tập trung vào tất cả các hàng mà function có giá trị là 1.
Ví dụ, ở hàng thứ 3 trong truth table ở trên, giá trị của hàm boolean là 1. 
Các giá trị tương ứng của mỗi biến là $x=0$, $y=1$, $z=0$, nên ta tạo một "term" $\bar{x}y\bar{z}$. Tương tự cho các hàng khác là hàng số 5 và hàng số 7 thì ta có $x\bar{y}\bar{z}$ và $x.y.\bar{z}$
Bây giờ, nếu chúng ta $+$ (Or) tất cả các *term* này (để cho tất cả các hàng mà function có giá trị 1), chúng ta nhận được một Boolean expression tương đương với truth table đã cho.
Do đó canonical representation của Boolean function trên là $f(x, y, z) = \bar{x}y\bar{z} + x\bar{y}\bar{z} + x.y.\bar{z}$. 
#### Two-Input Boolean Functions
Số lượng Boolean Function mà có thể được defined cho n (n ở đây là số lượng biến binary) là $2^{2^n}$. Ví dụ: Nếu có hai biến thì số lượng Boolean function có thể được defined là $2^(2*2) = 16$