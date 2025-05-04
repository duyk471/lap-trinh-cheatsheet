# Bài 1: Giới thiệu Java, JVM và Hellooo world

Đọc bài gốc của Loda đi nhá, cái này chỉ ghi lại một số ý để cho mình quay lại tham khảo thôi.

### Cài đặt môi trường

`Java` hoạt động như vậy, nó chỉ nói 1 ngôn ngữ duy nhất thôi, tuy nhiên nó có một thằng anh bá đạo, tên ông ý nôm na là môi trường ảo hay tên chuẩn là `Java virtual machine (JVM)`. Nhiệm vụ của `JVM` là nó phụ đề (thuyết minh) cho từng loại OS khác nhau rằng thằng `Java` đang làm gì, nói gì, làm gì.

Vì chúng ta là Developer nên sẽ cài gói `JDK` (`Java Development Kit`), nó chứa các công cụ giúp lập trình `Java`. Ngoài ra trong quá trình cài, nó sẽ cài môi trường `JRE` (`Java Runtime Enviroment`, bao gồm cả thằng `JVM` ở trên) luôn.

### Hello World

Tạo Project trong Intellij chẳng hạn, xong rồi thì cùng nhìn vào cấu trúc của project thì sẽ thấy có 3 thư mục:

- `.idea`: Thằng này là thư mục do `Intellij` tự tạo ra để chứa các file config của phần mềm này, bạn sẽ k cần quan tâm đến.
- `src`: Đây là thư mục chính bạn sẽ làm việc, tất cả `code` bạn để trong này
- `{project-name}.iml`: File này cũng do `Intellj` tạo ra và quản lý module, bạn không cần quan tâm nó.

### Một số thông tin khác

- `public static void main(String[] args)`: (Gọi tắt là `psvm` nhé) Cái thằng này sẽ là nơi `Java` tìm tới đầu tiên, và đọc toàn bộ các đoạn code trong cái thằng tên là `psvm` này. Dù nó ở bất cứ đâu, nó sẽ được tìm tới.
- 2 cái dấu `{``}`: Đánh dấu đoạn bắt đầu và kết thúc của cái `public static void main(String[] args)` kia.

Vậy là thằng `Java` sẽ đi lùng tìm, xem cái thằng `psvm` xem nó ở đâu. Rồi đọc hết tất cả những thứ nằm trong cái 2 dấu `{``}` của thằng này.

# Biến, phạm vi, kiểu dữ liệu, toán tử trong Java

### Biến & Kiểu dữ liệu

```java
public class Calculation{
    public static void main(String[] args){
        // khai bao so nguyen
        int a = 5;
        int b = 10;
        int x = 10 + 5;
        System.out.println(x);

    }
}
```

Thứ nhất là cái `// khai bao so nguyen`, cái này gọi là `Comment`, tức các bạn viết gì sau 2 cái dấu `//` thì nó sẽ không ảnh hưởng tới `code` của chương trình, nó chỉ mang ý nghĩa chú thích thôi.

Thứ hai là cái này:

```java
int a = 5;
```

Nói về `Biến` (`Variable`) các bạn có liên tưởng tới liên tưởng tới biến `x` trong đồ thị hàm số `ax + b = 0` không. Thì chính là nó đấy.

> Biến sẽ giúp chúng ta lưu trữ và quản lý các giá trị trong chương trình.

Trong `Java`, `Biến` cũng là đại diện cho một đối tượng và đối tượng này phải được xác định là thuộc `Kiểu dữ liệu` nào. Có các kiểu dữ liệu `nguyên thuỷ` (`primitive`) như sau:

- `boolean`: là kiểu logic, chỉ có 2 giá trị `true` hoặc `false`
- `char`: kiểu ký tự, chỉ chứa đc được một ký tự, được định nghĩa trong dấu ngoặc đơn `'`
- `int` : số nguyên (`1,2,3, ..`)
- `long`: số nguyên, lớn hơn `int`. (sẽ giải thích ở dưới)
- `float`: số thực (`1.5, 2.5, ..`).
- `double`: số thực, lớn hơn `float`.

Ngoài ra còn 2 kiểu dữ liệu nhỏ hơn `int` là `byte` và `short`.

Kiểu dữ liệu cao cấp hơn gọi là `Object` mà đặc trưng nhất là `String`.

- `String`: Một chuỗi các ký tự, được định nghĩa trong dấu ngoặc kép `""`. vd `String a = "Hellooo world~~~"`

### Cách khai báo

Để khai báo biến, bắt buộc trước đó bạn phải chỉ cho nó `kiểu dữ liệu` mà nó sẽ nhận, ngoài ra có thể có giá trị hoặc không.

- Cách 1: `[kiểu_dữ_liệu][tên_biến];`
- Cách 2: `[kiểu_dữ_liệu][tên_biến] = [giá_trị];`

```java
int a, b, c; // Khai báo 3 biến có kiểu dữ liệu int
float b = 4.5f, c = 4f; // Khai báo 2 biến có kiểu dữ liệu float với giá trị ban đầu. ở đây biến `c` sẽ được hiểu là c = 4.0
double c = 4444.3;
char t = 'c';
String e = "Hello";
```

### Cách đặt tên

Tên biến phải tuân theo `quy tắc lạc đà (Camel Case)`: đó là chữ cái đầu tiên của từ đầu tiên phải viết thường và chữ cái đầu tiên của các từ tiếp theo phải viết hoa, ví dụ: `listStudent`, `minScore`.

### Phạm vi sử dụng

Một khi bạn đã khai báo biến, thì bạn có thể sử dụng nó trong những `Phạm vi` mà nó khả dụng. ?? 😀?? Cùng nhìn ví dụ ở dưới nhé.

Ví dụ:

```java
public static void main(String[] args){
    // khai bao so nguyen `a`
    int a;
    // Gán giá trị cho a, bạn sử dụng toán tử `=`
    // Sử dụng biến a bình thường
    a = 124214;

    // lấy a và cộng thêm 1,, rồi gán ngược lại giá trị đó vào a :D
    // Sử dụng biến a bình thường
    a = a + 1;

}
// Gán lại giá trị cho a = 100 - 10;
// Chương trình lỗi
a = 100 - 10;
```

`Phạm vi` (`Scope`) là đây các bạn ạ, chính là 2 cái dấu `{}`, khi bạn khai báo một biến `a` trong 2 cái dấu `{``}` thì bạn chỉ có thể sử dụng ở trong nó thôi, ra ngoài nó sẽ không hiểu `a` là thằng nào và từ đâu chui ra.

> Biến không thể sử dụng ngoài, nhưng nó có thể được sử dụng ở bên trong những scope mà nó chứa hoặc cùng cấp với nó.

```java
public class Calculation{
    // Khai báo a ở ngoài main, cái `public static` là cần thiết nhé, còn chi tiết thì chúng ta sẽ học ở các bài sau.
    public static int a = 5;
    public static void main(String[] args){
        // thay đổi a, ở trong, vẫn okie.
        a = 10;

        // Biến a có thể sử dụng trong các `scope` con của nó
        // Làm gì biến a ở đây cũng được, biến đổi nó.

        // gán giá trị biến a vào b;
        int b = a;

        System.out.println(b);
    }
}
```

### Toán tử

Khi đã xác định các `Biến` trong chương trình, bạn có thể sử dụng `toán tử` để thay đổi các giá trị. Các `toán tử` thì khá đơn giản, giống môn toán bình thường thôi. Với các kiểu `nguyên thuỷ (primitive)` ta có:

```java
public class Calculation{
    public static void main(String[] args){
      int a;
      int b = 5;
      int c = a + b; // c = 0 + 5 cộng
      int d = a - b; // d = 0 - 5 trừ
      int f = a * 5; // f = 0 x 5 nhân
      int g = a / 5; // g = 0 : 5; chia
    }
}
```

Còn với `String` thì bạn có thể sử dụng `+` để ghép 2 chuỗi mà thôi. Còn các toàn tử còn lại không được sử dụng với `String`

```java
public class Calculation{
    public static void main(String[] args){
      String a = "Hello"
      String b = "World"
      // Mình đã nối 3 xâu là "Hello" + " " (Khoảng trắng) + "World" lại với nhau
      System.out.println(a + " " + b);

      String c = a + 5; // String cộng với một số nguyên?
      System.out.println(c); // Kết quả sẽ là: "Hello 5" :V
      // Bạn sẽ hiểu là khi cộng String với một số, số đó sẽ bị chuyển thành String và nối vào sau.

    }
}
```

### Ép kiểu dữ liệu

Nhìn vào ví dụ sau, bạn sẽ rõ.

```java
public class Calculation{
    public static void main(String[] args){
      int a = 2;
      float b = 3.5f; // dùng chữ f để nó hiểu đây là 3,5 float chứ k phải 3,5 double

      float c = a + b; // c = 5.5

      int d = a + b; // báo lỗi. Vì sao?
      // vì java đang hiểu 2 + 3.5 nó sẽ ép thành 5.5 là float. Bây giờ gán nó vào số nguyên thì sẽ như này int = float?

      // Để gán được bạn cần sử dụng ép kiểu
      int d = (int) a + b; // d = 5
      // a + b = 5.5 => ép thành (int) => 5 (lấy phần nguyên thôi)

      char character = '5';
      int number = (int) character; // number = 53. Why?

      // Vì ép `char` thành `int` thì nó sẽ không chuyển chữ thành số, mà nó sẽ kiếm tra '5' là ký tự ASCII thứ bao nhiêu trong máy tính, và trả lại số thứ tự đó.

      float = (float) 5; // => 5.0
    }
}
```

### Bản chất của biến (Nói thêm)

Khi các bạn khai báo một biến `int` trong chương trình của mình và sử dụng lung tung khắp mọi nơi, thì bạn có biết cái biến `int` ý ở đâu lòi ra không :))

Về bản chất, `Biến` sẽ là một vùng nhớ trong thiết bị vật lý mà dễ nhất là để trong `ram`. và khi bạn cho nó một giá trị, nó sẽ lưu trữ số đó vào `ram`, và cần thì lấy lên.

Vậy để `ram` biết bạn muốn lưu cái gì thì bạn phải khai báo cho nó. Ví dụ bạn bảo tôi cần một số nguyên `int`. Thì máy tính hiểu là mình cần lưu trữ một số nguyên bình thường, không quá lớn, nó sẽ cho bạn `4 byte` trong `Ram` thích lưu gì thì lưu. nhưng `không được vượt quá 4 byte`.

> 4 byte = 32 bit, bỏ đi 1 bit đầu tiên để đánh dấu là số âm hay dương, thì còn 31 bit => số lớn nhất mà biến int lưu trữ được là 2^31 - 1 = 2147483647

Từ đây, bạn sẽ hiểu vì sao có số `long`, vì nhu cầu lưu số lớn hơn thì `long` được cấp tận `8 byte`.

Còn trường hợp đặc biệt như `String` thì tuỳ giá trị của nó có bao nhiêu ký tự, mà `Ram` sẽ cấp tương ứng bấy nhiêu `byte`

# Bài 3: Hàm và câu lệnh điều kiện


## #1 Câu lệnh rẽ nhánh

### if

Các bạn nhìn qua ví dụ này:

```java
public static void main(String[] args){
    // khai bao so nguyen
    int a = 9;

    // Kiểm tra xem a có bằng 9 không
    if (a == 9) {
        // nếu bằng 9, in ra màn hình "Hello"
        System.out.println("Hello");
    }

// Kết quả trên màn hình:
// Hello
}
```

Thì các cần biết như sau, câu lệnh `if` là một câu lệnh điều kiện, và nhận vào là một điều kiện `true` hoặc `false`. Có cú pháp như sau:

```java
if ([điều kiện]){
    // Thực hiện đoạn code nếu [điều kiện] là `true`. Nếu `false` bỏ qa đi xuống dưới.
}
// Tiếp tục thực hiện đoạn code phía dưới
```

Vậy đấy, nên để so sánh bạn cần dùng `toán tử quan hệ` mình liệt kê dưới đây:

- `==`: Kiểm tra 2 toán hạng có `bằng nhau` không? (`if(a==b)`)
- `!=`: Kiểm tra 2 toán hạng có `khác nhau` không? (`if(a!=b)`)
- `>`: Kiểm tra toàn hạng A có `lớn hơn` B không? (`if(a>b)`)
- `<`: Kiểm tra toàn hạng A có `nhỏ hơn` B không? (`if(a<b)`)
- `>=`: Kiểm tra toàn hạng A có `lớn hơn hoặc bằng` B không? (`if(a>=b)`)
- `<=`: Kiểm tra toàn hạng A có `nhỏ hơn hoặc bằng` B không? (`if(a<=b)`)

Tất cả `toán tử quan hệ` ở trên, khi thực hiện xong nó sẽ trả về là kiểu `boolean`, nên bạn có thể gán nó vào một biến bất kỳ, như lày:

```java
int a = 5;
int b = 6;

boolean result = a == b; // false

System.out.println("Result: " + result);

// Kết quả in ra trên màn hình:
// "Result: false"

if(result){ // viết tắt của if(result == true)
    System.out.println("Result is true");
}
```

Đến đây, có thể nói câu lệnh `if` thực chất nhận vào một giá trị `boolean`.

### else

Tiếp theo, chúng ta tới với dạng đầy đủ của `if` chính là cấu trúc `if else`.

```java
if ([điều kiện]){
    // Thực hiện đoạn code nếu [điều kiện] là `true`.
} else {
    // Thực hiện đoạn code trong này nếu [điều kiện] là `false`
}
//Các đoạn code ở dưới thực hiện bình thường sau khi if hoặc else diễn ra
```

Ví dụ:

```java
int a = 5;
if ( (a + 2) == 7 ){
    System.out.println("Bằng 7");
    // Sử dụng biến `a` ở ngay trong scope {} của `if`,, như bài #2 mình có nói, biến được sử dụng trong các scope con hoặc bằng cấp
    System.out.println("Giá trị lúc này của a = " + a);
}else{
    System.out.println("Khác 7");
    System.out.println("Giá trị lúc này của a = " + a);
    int b = 7; // Tạo ra 1 biến b trong else
}

b = 50; // Lỗi, không biết b là gì, vì b ở scope nhỏ hơn, bên ngoài không hiểu.
```

### Toán tử logic

Toán tử logic là những toán tử giúp chúng ta kết hợp nhiều [điều kiện] lại với nhau.

Ví dụ mình nói: `"Nếu ab = 3 VÀ ac = 4 VÀ bc = 5 thì abc là tam giác vuông"`

Thì trong code cần viết chương trình như thế nào?

Cách 1: Sử dụng `if` thông thường.

```java
int ab = 3;
int ac = 4;
int bc = 5;

if(ab == 3){
    if(ac == 4){
        if(bc == 5){
            System.out.println("abc là tam giác cực vuông");
        }
    }
}
```

Cách 2: Sử dụng `if` và `toán tử logic`

```java
int ab = 3;
int ac = 4;
int bc = 5;

// Nếu ab = 3 VÀ ac = 4 VÀ bc = 5
if(ab == 3 && ac == 4 && bc==5){
    // thì abc là tam giác vuông
    System.out.println("abc là tam giác cực vuông");
}
```

Các bạn nhìn ví dụ cũng đoán ra `&&` chính là `toán tử logic` đại diện cho khái niệm `AND`. Chúng ta có tất cả các loại `toán tử logic` như sau:

- `&&`: AND
- `||`: OR
- `!`: NOT

Mục tiêu của các `toán tử logic` là tác động lên các biểu thức `boolean` để cho ra mộ biến `boolean` mới.

### Phép AND (&&)

Phép `&&` hoạt động theo nguyên tắc, `chỉ cần có 1 cái sai, thì tất cả đều sai` hay `Tất cả đều phải đúng, mới là đúng`

Nếu `"A đúng và B đúng và C sai thì kết quả vẫn là sai"`

```java
// Bạn chạy thử xem nó đi vào phần nào nhé
if(true && true && true && false){
    System.out.println("true");
}else{
    System.out.println("false");
}
```

### Phép OR (||)

Phép `||` thì rất dễ dãi, `Chỉ 1 cái đúng là đủ`

```java
// Bạn chạy thử xem nó đi vào phần nào nhé
if(false || false || true || false){
    System.out.println("true");
}else{
    System.out.println("false");
}
```

### Phép NOT (!)

Phép `!` làm phủ định giá trị của biểu thức, nếu nó đang `true` thì biến nó thành `false` và ngược lại.

```java
int a = 7;
if(!(a == 7)){ // (a==7) => true gặp thằng ! lại bị chuyển thành false. => vào vế else
    System.out.println("Đáng nhẽ ra nên vào đây");
}else{
    System.out.println("But nope, nó lại vào đây");
}
```

## Hàm (Function)

```java
public class Calculation {
    public static void main(String[] args){
        f(5,6);
        f(2,3);
        f(1,10);
    }

    public static void f(int x, int y){
        int a = x + y;
        System.out.println("In a ra màn hình: " + a);
    }
}
// Kết quả khi chạy:

// In a ra màn hình: 11
// In a ra màn hình: 5
// In a ra màn hình: 11
```

### Cách khai báo

Cách khai báo một phương thức như sau:

`[kiểu_truy_cập] [kiểu_trả_về] [tên_phương_thức] ([danh_sách_tham_số]){}`

ví dụ:

```java
public static void f(int x, int y){
    // Code của bạn
}

public static void main(String[] args){

}
```

Và khai báo ở ngoài hàm `main()`. Tới đây, bạn hiểu `main()` cũng là một `hàm (function)`. Tuy nhiên nó đặc biệt vì cú pháp của nó là cố định và được `Java` tìm tới để đọc đầu tiên.

1 - `[kiểu_truy_cập]`:

Trong ví dụ trên `[kiểu_truy_cập]` chính là vế `public static`. Nó định nghĩa phạm vi `hàm` được sử dụng. chúng ta sẽ tìm hiểu ở các bài sau nhé các bạn, bây giờ bạn hãy mặc định sử dụng `public static` ở trước mỗi hàm khai báo để có thể sử dụng được nhé. Ở bài này, chúng ta tạm hiểu với nhau: `public static` là `"truy cập ở bất cứ đâu"` tức có thể gọi hàm này ở bất kì chỗ nào.

2 - `[kiểu_trả_về]`:

Tương đương với phần `void` ở ví dụ trên, kiểu trả về là giá trị chúng ta nhận được sau khi gọi hàm.

Bạn hãy nhớ lại, khi truyền `x` vào `f(x)` chúng ta sẽ nhận lại là `y`. Thì hàm cũng vậy, chúng ta có thể trả lại một giá trị gì đó. ví dụ:

```java
// [kiểu trả về]: int
public static int tong(int x, int y){
    int t = x + y; // Tính tổng 2 só x, y
    return t; // trả số đó ra sử dụng câu lệnh `return {biến}`
}

public static void main(String[] args){
    int t = tong(5,6); // Lấy giá trị trả ra, gán nó vào t;
}
```

Tôi định nghĩa một hàm tính tổng `tong(x,y)` nhận vào 2 số nguyên, và yêu cầu nó trả ra một số `int`.

Các kiểu trả về:

- `primitive`: `int`, `boolean`, `char`, ...
- `Object`: `String`, (còn rất nhiều, sẽ học ở bài tiếp theo)
- `void`: Không trả về gì cả

Ở ví dụ đầu tiên mình đã sử dụng `void` để định nghĩa hàm.

```java
public static void f(int x, int y){
    int a = x + y;
    System.out.println("In a ra màn hình: " + a);
}
```

Điều này nói là hàm của chúng ta thực hiện một hoạt động khép kín, và không có nhu cầu trả ra ngoài cái gì cả. Mình chỉ tính tổng rồi in luôn ra màn hình thôi, không cần đưa gì ra ngoài cả.

3 - `[danh_sách_tham_số]`

Tham số đầu vào, là những thứ chúng ta đưa vào hàm, định nghĩa tham số đầu vào bao gồm `[kiểu_dữ_liệu] [tên_biến]`. Chúng ta có truyền nhiều tham số vào `hàm` bằng cách đặt dầu phẩy `,` giữa mỗi tham số.

```java
public static int f(int x, int y, int z, ... ){
    // code
}
```

Ở đây lưu ý phần `[tên_biến]` bạn có thể đặt tên bất kỳ. chẳng hạn:

```java
// Hàm nhận vào 2 biến `x`, `y` và trả ra kết quả `boolean` xem nó có bằng nhau hay không
public static boolean bangnhau(int x, int y){
    return x == y;
}

public static void main(String[] args){
    int a = 5; // tên biến là `a`
    int b = 6; // tên biến là `b`

    boolean ketqua = bangnhau(a,b); // đưa `a` , `b` vào hàm.
    // bản chất khi gọi hàm `bangnhau`:
    // int x = a;
    // int y = b;
    // return x == y;
    //
    System.out.println("Kết quả: " + ketqua);
}
```

Bạn định nghĩa `tham số đầu vào` là `x` và `y` thì nó chỉ hiểu trong ở hàm đó thôi, và những giá trị truyền vào sẽ gán vào các biến `x` và `y`.

# Nhập xuất dữ liệu trong Java

### Nhập xuất từ bàn phím

```java
public class Calculation {
    public static void main(String[] args) {
        // Chúng ta khai báo 3 biến a,b,c không có giá trị.
        int a, b, c;

        //Khai báo đối tượng Scanner, giúp chúng ta nhận thông tin từ keyboard
        Scanner sc = new Scanner(System.in);
        System.out.print("Nhập a: "); //print thay vì println, nó sẽ in ra, nhưng không xuống dòng

        a = sc.nextInt(); // sc.nextInt() là cách để lấy giá trị từ bàn phím, nó sẽ chờ tới khi chúng ta nhập một số.
        System.out.print("Nhập b: ");
        b = sc.nextInt();
         System.out.print("Nhập c: ");
        c = sc.nextInt();
        // In các giá trị ra màn hình
        System.out.println("a = " + a + ", b = " + b + ", c = " + c);
        //  Đây là phép cộng String mình đã nói trong Bài #1.
}
```

Cái dòng lệnh này `a = sc.nextInt()`. Nó sẽ chờ cho tới khi bạn nhập 1 số nguyên và gõ `Enter` thì thôi. Giả sử mình nhập `5`


Chương trình lại tiếp tục chạy cho tới khi gặp câu lệnh `sc.nextInt()` tiếp theo. Và cứ tiếp tục như vậy cho tới dòng lệnh cuối cùng.

Từ đây, các bạn có thể hiểu là đối tượng `Scanner` đã làm nhiệm vụ là nhận dữ liệu người dùng nhập từ bàn phím, và gán nó vào biến, bằng câu lệnh `nextInt`.

Bây giờ quay trở ngược lên trên 1 chút, ở câu lệnh:

```java
Scanner sc = new Scanner(System.in);
```

các bạn sẽ thấy một khái niệm là `new`. cái này thì [Bài #5][link-bai5] mình sẽ nói chi tiết, còn ở đây thì bạn hiểu nó được sử dụng để tạo ra 1 đối tượng `Scanner`. 

### Các phương thức nhập xuất

`Scanner` có một loạt các hàm hỗ trợ như sau:

- `next()`: Nhận vào một `String token` (nhận vào 1 từ đầu tiên thay cả câu)
- `nextInt()`: Nhận vào một số `int`
- `nextLong()`: Nhận vào một số `long`
- `nextFloat()`: Nhận vào một số `float`
- `nextDouble()`: Nhận vào một số `double`
- `sc.nextLine()`: Nhận vào một `chuỗi String` (Cả 1 câu)
- `nextByte()`: Nhận vào một `byte`
- `nextBoolean()`: Nhận vào một `boolean`

Các hàm trên bạn hiểu nguyên lý là nó đều sẽ `chờ` cho tới khi bạn nhập kiểu dữ liệu nó muốn vào.

Có `next()` và `nextLine()` khá đặc biệt, mình sẽ ví dụ:

```java
Scanner sc = new Scanner(System.in); //Tạo đối tượng Scanner
System.out.print("Nhập gì đó: ");
String a = sc.nextLine(); // nhận vào 1 string
System.out.println("Bạn vừa nhập: "+a);

System.out.print("Nhập thêm gì đi: ");
String b = sc.next(); // cũng nhận vào 1 String
System.out.println("Bạn vừa nhập: "+b);
```

`nextLine` thì nhận vào cả 1 chuỗi dài `String`, cho tới khi bạn nhấn `Enter`. Còn `next` dù bạn có nhập dài như nào, nó cũng nhận 1 từ đầu tiên thôi.

### Bản chất của`next`

Bạn để ý là các hàm lấy giá trị từ bàn phím đều có chữ `next`. Bây giờ bạn chạy cho mình ví dụ này, bạn sẽ hiểu:

```java
public static void main(String[] args) {
    int a,b,c;
    Scanner sc = new Scanner(System.in); // Tạo đối tượng Scanner
    System.out.print("Nhập a: ");
    a = sc.nextInt();
    b = sc.nextInt();
    c = sc.nextInt();
    System.out.println("a = "+a);
    System.out.println("b = "+b);
    System.out.println("c = "+c);
}
```

Bạn sẽ thấy là, nó đưa tuần tự các giá trị hiện có trên bàn phím vào các biến. bản chất của chữ `next` chính là tuần tự. Nó sẽ chờ bạn nhập nếu không có giá trị gì trên màn hình, nhưng nếu đã có sẵn giá trị rồi, nó sẽ ghi nhớ trong `bộ đệm` và khi gặp hàm `nextInt()` nó không chờ nữa, mà nó lấy luôn cái giá trị còn thừa ra, chưa sử dụng đến để gắn luôn vào biến 😂

Nhìn như như này cho dễ hiểu:

```java
public static void main(String[] args) {
    int a,b,c;
    Scanner sc = new Scanner(System.in); // Tạo đối tượng Scanner
    System.out.print("Nhập a: ");
    a = sc.nextInt(); // Chờ bạn nhập.
    // bạn nhập: 5 6 7 8 9 10
    // bộ đệm = 5 6 7 8 9 10
    // lấy 5 ra, gắn vào a
    // bộ đệm còn: 6 7 8 9 10
    b = sc.nextInt(); // gặp lệnh nextInt()
    // thấy bộ đệm còn, lấy 6 ra, gắn vào b
    // bộ đệm còn: 7 8 9 10
    c = sc.nextInt(); // gặp lệnh nextInt()
    // thấy bộ đệm còn thừa, lấy 7 ra, gắn vào b
    // bộ đệm còn: 8 9 10
    System.out.println("a = "+a); // in a
    System.out.println("b = "+b); // in b
    System.out.println("c = "+c); // in c
}
```

### Inpụt/ outpụt từ File

Để cho thuận tiện trong việc đọc ghi, thì ngoài bàn phím, một trong những yêu cầu quan trọng khi lập trình đó là nhập xuất dữ liệu từ File, phần này sẽ không khác nhiều với từ bàn phím đâu các bạn, mình sẽ hướng dẫn.

Tại thư mục gốc của project, bạn click `New` > `File`. Tạo 1 tệp tên là `input.txt`. Như hình:


```java
public static void main(String[] args) throws FileNotFoundException { // Thêm cái này vào đây
    int a,b,c;
    Scanner sc = new Scanner(new File("input.txt")); // Tạo đối tượng Scanner đọc tới cái file vừa tạo
    System.out.print("Nhập a: ");
    a = sc.nextInt();
    b = sc.nextInt();
    c = sc.nextInt();
    System.out.println("a = "+a); // in a
    System.out.println("b = "+b); // in b
    System.out.println("c = "+c); // in c
}
// Kết quả chạy:
// Nhập a: a = 5
// b = 7
// c = 8
```

Đoạn `throws FileNotFoundException`. Ở đây thì bạn hiểu nó là lỗi có thể xảy ra, nếu nó không tìm thấy file `input.txt` thì nó sẽ xảy ra cái lỗi kia. Chúng ta sẽ xử lý lỗi đó sau, hiện tại thì nếu bạn nhập đúng tên File thì không thể lỗi được.# Vì sao nên sử dụng StringBuffer

Cùng xem ví dụ này nhé:

```java
long start = System.nanoTime();

String s = "Hello";
for (int i = 0; i < 1000; i++) {
    s += " world";
}
long end = System.nanoTime();
System.out.println("Total time: "+(end-start));

// Kết quả:
// Total time: 17495917 ns
// = 17.4 ms (Milliseconds)
```

Bây giờ, vẫn là chương trình tương tự, mình sử sụng `String Buffer`

```java
long start = System.nanoTime();

StringBuffer sb = new StringBuffer("Hello");
for (int i = 0; i < 1000; i++) {
    sb.append(" world");
}
String s = sb.toString();
long end = System.nanoTime();
System.out.println("Total time: "+(end-start));

// Kết quả:
// Total time: 461198 ns
// = 0.46 ms
```

`String Buffer` nhanh hơn gấp 38 lần.

Hiệu năng được chạy trên Mac Pro 2017, tại máy bạn có thể sẽ khác, nhưng chắc chắn rằng `StringBuffer` luôn nhanh hơn!

### Góc giải thích

Có một điều ít bạn học lập trình `Java` để ý, đó là `String` là `immutable`. Tức nội dung trong `String` là không được quyền thay đổi.

Nhiều bạn lầm tưởng rằng việc nối xâu là bạn thay đổi nội dung của `String`, nhưng thực chất bạn đang tạo ra một đối tượng hoàn toàn mới:

```java
String s = "A";
s += "B";
// Complier sẽ tạo ra một đối tượng mới là "AB"
// Và gán vào `s`
// Bản chất `s` bây giờ là một đối tượng mới chứ bạn không hề thay đổi nội dung ban đầu của `s`.
// Đây là những gì ở dưới Compiler sẽ làm:
StringBuffer sb = new StringBuffer("A"); // Compiler Vẫn phải xài tới StringBuffer
sb.append("B");
s = sb.toString();
```

Vì vậy khi nối xâu trong `Java`, việc bạn thực hiện nó liên tục, sẽ tương đương với việc khởi tạo liên tục và nối 2 xâu lại rồi trả về đối tượng `String` mới dẫn tới chi phí lớn.

`StringBuffer` cho phép chúng ta thao tác trên một đối tượng duy nhất và thay đổi được nội dung trong nó. Nếu ban đầu nội dung là `"A"`, bạn muốn nối thêm `"B"` vào. Thì nó chỉ cần gắn chuỗi `bytes` của `"B"` vào liền kề ngay sau `"A"` là xong. (Vì nó có thể thay đổi, khác với `String` là `immutable`).


# Hướng dẫn Java Reflection

### Giới thiệu

`Java Relection` là một core package trong thư viện chuẩn của `Java`. Mục đích của nó là cho phép chúng ta truy cập vào gần như mọi thứ bên trong đối tượng. "Dưới một góc độ khác"!

Chúng ta thường biết tới `Java` thông qua khái niệm hướng đối tượng như sau:

```java
String str = "Hello Loda";
str.toUpperCase(); // Chúng ta gọi hàm toUpperCase() thông qua toán tử "."
// Mọi thứ trong đối tượng là khép kín, chúng ta phải gọi thông qua hàm public
```

Hoặc

```java
public class Girl {
    String name;
    int age;
    int atk;
    int agi;
    int def;
    // ... Và 1000 thuộc tính khác

    public static void main(String[] args) {
        Girl girl = new Girl();
        // Chúng ta thường phải nhớ tên thuộc tính để gọi nó ra
        girl.name = "Ngoc Trinh";

        // Giá sử class này có 100 thuộc tính là String.
        // Bạn muốn set giá trị của tất cả trường String là "Ngoc Trinh"
        // Bạn sẽ rất bối rối vs việc gọi từng thuộc tính bằng việc ".{tên thuộc tính}" như này.

        // Có cách nào cho code duyệt tìm toàn bộ thuộc tính, cái nào là String thì đổi nó thành "Ngoc Trinh"?
    }
}
```

Đúng vậy, khi chúng ta muốn gọi tên thuộc tính, mà lại không muốn gõ `.` và nhớ ra tên thuộc tính, thì làm như nào?

Bây giờ, chúng ta phải tiếp cận từ góc nhìn khác. Chúng ta sẽ ước mình có thể duyệt hết tất cả các thuộc tính của 1 class bằng vòng lặp. Rồi check xem thuộc tính có là `String` không? nếu có thì gán giá trị mới là "Ngoc Trinh"!

Để làm được điều này, chúng ta cần đào sâu vào `Class` và phá vỡ giới hạn của java truyền thống. Đây là lúc `Java Reflection` (Sự phản chiếu) vào trận.

### Java Reflection

`Java Reflecion` cho phép bạn đánh giá, sửa đổi cấu trúc và hành vi của một đối tượng tại thời gian chạy (runtime) của chương trình. Đồng thời nó cho phép bạn truy cập vào các thành viên private (private member) tại mọi nơi trong ứng dụng, điều này không được phép với cách tiếp cận truyền thống.

### Lấy ra Thuộc tính (Field)

Quay trở lại ví dụ trên, Chúng ta sẽ lấy ra toàn bộ thuộc tính của `Girl`. Tìm xem cái nào tên `name` và bổ sung giá trị mới cho nó.

```java
public class Girl {
    private String name;

    public Girl() {

    }

    public Girl(String name) {
        this.name = name;
    }

    public void setName(String name){
        this.name = name;
    }

    @Override
    public String toString() {
        return "Girl{" +
               "name='" + name + '\'' +
               '}';
    }

    public static void main(String[] args) throws Exception {
        Girl girl = new Girl(); // KHởi tạo đối tượng Girl
        girl.setName("Ngoc trinh");

        // Lay ra tat ca field cua object
        // Chỉ để bạn xem ví dụ thôi, bỏ qua phần này nhé!
        for(Field field : girl.getClass().getDeclaredFields()){
            System.out.println();
            System.out.println("Field: " +field.getName());
            System.out.println("Type: " +field.getType());
        }

        // PHẦN CHÍNH
        Field nameField = girl.getClass().getDeclaredField("name"); // Lấy ra field có tên "name" (nếu không tìm thấy, nó sẽ bắn NoSuchFieldException)
        nameField.setAccessible(true); // Cho phép truy cập tạm thời. (Vì nó đang là Private mà)

        // Bây giờ cái "nameField" đại diện cho thuộc tính "name" của mọi object có class Girl.
        nameField.set(girl, "Bella"); // thay giá trị mới của `girl` bằng nameField.

        System.out.println(girl);
    }
}
// Output:
// Field: name
// Type: class java.lang.String
// Girl{name='Bella'}
```

### Lấy ra Hàm (Method)

Vấn đề đặt ra, giống với `field`. Chúng ta cũng sẽ có nhu cầu duyệt tìm một `method` nào đó và sử dụng nó:

```java
public static void main(String[] args) throws Exception {
    Class<Girl> girlClass = Girl.class;

    // Su dung getDeclaredMethods de lay ra nhung method cua class va cha no.
    Method[] methods = girlClass.getDeclaredMethods();
    for(Method method : methods){
        System.out.println();
        System.out.println("Method: " + method.getName());
        System.out.println("Parameters: " + Arrays.toString(method.getParameters()));
    }

    // Lay ra method ten la setName va co 1 tham so truyen vao ->
    // => chính là: setName(String name)
    Method methodSetName = girlClass.getMethod("setName", String.class);
    // Bây giờ methodSetName sẽ đại diện cho method setName(String name) của mọi object có class là Girl

    Girl girl = new Girl(); // Tạo ra đối tượng Girl

    // Thực hiện hàm setName() trên đối tượng girl, giá trị truyền vào là "Ngoc Trinh"
    methodSetName.invoke(girl, "Ngoc Trinh");
    System.out.println(girl);
}
```

### Lấy ra Constructor

Lấy ra hàm khởi tạo của một class. Từ đó cho phép chúng ta cách tạo ra đối tượng từ theo một cách khác, thay vì `new Class()` như bình thường

```java
public static void main(String[] args) {
    Class<Girl> girlClass = Girl.class;
    System.out.println("Class: " + girlClass.getSimpleName());
    System.out.println("Constructors: " + Arrays.toString(girlClass.getConstructors())); // Lấy ra toàn bộ Constructor của class này
    try {
        // Tạo ra một object Girl từ class. (Khởi tạo không tham số)
        Girl girl1 = girlClass.newInstance();
        System.out.println("Girl1: " + girl1);

        // Lấy ra hàm constructor với tham số là 1 string
        // Chính là -> public Girl(String name) {}
        Constructor<Girl> girlConstructor = girlClass.getConstructor(String.class);
        Girl girl2 = girlConstructor.newInstance("Hello");

        System.out.println("Girl2: " + girl2);
    } catch (Exception e) {
        // Exception xay ra khi constructor khong ton tai hoac tham so truyen vao khong dung
        e.printStackTrace();
    }
}
```

### Lấy ra Annotation trên Field, Method, Class

Đúng vậy, đây cũng chính là một trong những phần quan trọng bậc nhất của `Java Reflection`. Cho phép chúng ta kiểm tra `Class` hiện tại đang được chú thích bởi những `Annotation` nào.

```java
@SuppressWarnings("deprecation")
@Deprecated
public class Girl {
    private String name;

    public Girl() {

    }

    public Girl(String name) {
        this.name = name;
    }

    @Nullable
    public void setName(String name){
        this.name = name;
    }

    @Override
    public String toString() {
        return "Girl{" +
               "name='" + name + '\'' +
               '}';
    }

    public static void main(String[] args) {
        Class<Girl> girlClass = Girl.class;
        System.out.println("Class: "+girlClass.getSimpleName()); // Lấy ra tên Class
        for(Annotation annotation : girlClass.getDeclaredAnnotations()){
            System.out.println("Annotation: " + annotation.annotationType()); // Lấy ra tên các Annatation trên class này
        }

        for(Method method: girlClass.getDeclaredMethods()){ // Lấy ra các method của class
            System.out.println("\nMethod: " + method.getName()); //Tên method
            for(Annotation annotation : method.getAnnotations()){
                System.out.println("Annotation: " + annotation.annotationType()); // Lấy ra tên các Annatation trên method này
            }
        }
    }
}
```
# Hướng dẫn tự tạo một Annotations

### Khái niệm

`Annotation` (Chú thích) được sử dụng để chú thích trên một `class`, một trường (`field`) hoặc một `method` để cung cấp hoặc bổ sung các thông tin. Nó hoàn toàn không ảnh hưởng tới code của bạn.

Trong bài có sử dụng các kiến thức:

1. Optional
2. Functional Interface & Lambda
3. Java Reflection

`Annotation` được sử dụng ở 3 dạng:

- Chú thích cho trình biên dịch (Compiler)
- Chú thích cho quá trình build
- Chú thích trong quá trình chạy chương trình (Runtime)

Hẳn bạn đã 1 lần từng thấy cái `@Override` phải không? nó là một _Annotation chú thích cho trình biên dịch_, để cho trình biên dịch biết hàm đó đã bị ghi đè.

Còn _chú thích cho quá trình build_ thì không hẳn có ví dụ cụ thể, nhưng bạn hãy nghĩ tới `Maven`, `Gradle` những công cụ build này sẽ có thêm thông tin khi build ứng dụng của bạn khi gặp một số `Annotation` đặc biệt, và sẽ bổ sung thêm code vào đó.

_Chú thích trong quá trình chạy chương trình_ sẽ là nội dung chính của chúng ta hôm nay. Đây là những `Annotation` mà chỉ khi bạn chạy chương trình rồi thì nó mới tác động tới code. Cùng vào ví dụ để dễ hiểu nhé!

### Khai báo Annotation

Cách khai báo `Annotation` là sử dụng `@interface`

vậy là bạn đã có 1 `Annotation`. Giờ gọi nó ra và sử dụng:

### Khai báo phạm vi cho Annotation

Chúng ta có thể quy định phạm vi sử dụng của `Annotation` bằng cách:

`@Retention`: Dùng để chú thích mức độ tồn tại của một annotation nào đó. Cụ thể có 3 mức nhận thức tồn tại của vật được chú thích:

1. `RetentionPolicy.SOURCE`: Tồn tại trên code nguồn, và không được bộ dịch (compiler) nhận ra.
2. `RetentionPolicy.CLASS`: Mức tồn tại được bộ dịch nhận ra, nhưng không được nhận biết bởi máy ảo tại thời điểm chạy (Runtime).
3. `RetentionPolicy.RUNTIME`: Mức tồn tại lớn nhất, được bộ dịch (compiler) nhận biết, và máy ảo (jvm) cũng nhận ra khi chạy chương trình.

`@Target`: Dùng để chú thích phạm vi sử dụng của một `Annotation`

1. `ElementType.TYPE` \- Cho phép chú thích trên Class, interface, enum, annotation.
2. `ElementType.FIELD` \- Cho phép chú thích trường (field), bao gồm cả các hằng số enum.
3. `ElementType.METHOD` \- Cho phép chú thích trên method.
4. `ElementType.PARAMETER` \- Cho phép chú thích trên parameter
5. `ElementType.CONSTRUCTOR` \- Cho phép chú thích trên constructor
6. `ElementType.LOCAL_VARIABLE` \- Cho phép chú thích trên biến địa phương.
7. `ElementType.ANNOTATION_TYPE` \- Cho phép chú thích trên Annotation khác
8. `ElementType.PACKAGE` \- Cho phép chú thích trên package.

### Xử lý Annotation

Bước 1: Chú thích bất kì chỗ nào bạn muốn.

Bước 2: Viết class xử lý `@JsonName`

Bước 3: Chạy thử:

```java
public @interface JsonName {
    String value(); // các giá trị trong @interface đều dạng hàm abstract, không tham số
}
```

```java
@JsonName(value = "super_man")
public class SuperMan extends Person {
    private String name;
}
```

```java
@Retention(RetentionPolicy.RUNTIME) // Tồn tại trong lúc chạy chương trình
@Target({ ElementType.TYPE, ElementType.FIELD, ElementType.METHOD}) // Được sử dụng trên class, interface, method, biến
public @interface JsonName {
    String value();
}
```

```java
@JsonName(value = "super_man")
public class SuperMan {
    // Không chú thích, thì chúng ta sẽ coi như lấy tên field là `name` luôn
    private String name;

    @JsonName("date_of_birth")
    private LocalDateTime dateOfBirth;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public LocalDateTime getDateOfBirth() {
        return dateOfBirth;
    }

    public void setDateOfBirth(LocalDateTime dateOfBirth) {
        this.dateOfBirth = dateOfBirth;
    }
}
```

```java
public class JsonNameProcessor {
    public static String toJson(Object object) throws IllegalAccessException {
        StringBuilder sb = new StringBuilder(); // Dùng StringBuilder de tao json tu class

        Class<?> clazz = object.getClass();
        JsonName jsonClassName = clazz.getDeclaredAnnotation(JsonName.class); // Lay ra annotation @JsonName tren Class

        sb.append("{\n")
          .append("\t\"")
          // Lay gia tri cua Annotation, neu annotation la null thi lay ten Class de thay the
          .append(Optional.ofNullable(jsonClassName).map(JsonName::value).orElse(clazz.getSimpleName()))
          .append("\": {\n"); //

        Field fields[] = clazz.getDeclaredFields();
        for (int i = 0; i < fields.length; i++) {
            fields[i].setAccessible(true); // Set setAccessible = true. De co the truy cap vao private field
            JsonName jsonFieldName = fields[i].getDeclaredAnnotation(JsonName.class); // get annotation tren field
            sb.append("\t\t\"")
              // Lay gia tri cua Annotation, neu annotation la null thi lay ten field thay the
              .append(Optional.ofNullable(jsonFieldName).map(JsonName::value).orElse(fields[i].getName())) // L
              .append("\": ")
              // Neu field la String hoac Object. thi append dau ngoac kep vao
              .append(fields[i].getType() == String.class || !fields[i].getType().isPrimitive() ? "\"" : "")
              // Lay gia tri cua field
              .append(fields[i].get(object))
              // Neu field la String hoac Object. thi append dau ngoac kep vao
              .append(fields[i].getType() == String.class || !fields[i].getType().isPrimitive()? "\"" : "")
              // Nếu là field cuối cùng, thì không append dấu ","
              .append(i != fields.length -1 ? ",\n" : "\n");
        }
        sb.append("\t}\n");
        sb.append("}");

        return sb.toString();
    }
}
```

```java
public static void main(String[] args) throws IllegalAccessException {
    SuperMan superMan = new SuperMan(); // Tao doi tuong super man
    superMan.setDateOfBirth(LocalDateTime.now());
    superMan.setName("loda");

    String json =JsonNameProcessor.toJson(superMan);
    System.out.println(json);
}
// OUTPUT:
/*
{
	"super_man": {
		"name": "loda",
		"date_of_birth": "2019-04-03T21:07:23.983"
	}
}
*/
```

# 「Java 8」Functional Interfaces & Lambda Expressions cực dễ hiểu

### Giới thiệu

Khái niệm `Functional Interfaces` được `Java` đưa ra cùng với phiên bản `Java 8`. về cơ bản, có thể hiểu:

> Functional Interfaces là interface nhưng chỉ có một 1 abstract function duy nhất.

Ví dụ:

```java
interface Runable {
    public void run(); // Chỉ có duy nhất một abstract function.
}
```

### Functional Programming

Trước khi đi vào chi tiết, chúng ta cùng tìm hiểu khái niệm `Lập trình hướng hàm`.

Cùng xem ví dụ dưới đây:

```java
public static void main(String[] args) {
    // Mình muốn xử lý dữ liệu trước khi ỉn ra màn hình.
    System.out.println(process("Hey Loda!!!"));
}

public static String process(String input){
    // Cho tất cả viết hoa lên.
    return input.toUpperCase();
}

// Output:
HEY LODA!!!
```

Tuy nhiên bạn sẽ thấy cách làm này không `flexible`, vì các bạn chỉ có thể xử lý cho chữ thành `UPPER CASE`. Muốn làm gì đó khác, như `toLowerCase` chẳng hạn, mình sẽ phải viết một `function` mới.

Chúng ta giải quyết cách cách này bằng `Anonymous function (Hàm ẩn danh)`

Sửa code chút:

```java
public interface StringProcessor{
    public String process(String input);
}

// StringProcessor ở đây là một Interface, hay Functional Interface
public static String getStr(String input, StringProcessor processor){
    return processor.process(input);
}

public static void main(String[] args) {
    // In ra chữ hoa
    System.out.println(getStr("Hello Loda!", new StringProcessor() {
        @Override
        public String process(String input) {
            return input.toUpperCase();
        }
    }));

    // In ra chữ thường
    System.out.println(getStr("Hey Loda!", new StringProcessor() {
        @Override
        public String process(String input) {
            return input.toLowerCase();
        }
    }));
}
// Output:
// HELLO LODA!
// hey loda!
```

### Lambda Expressions

Quay lại ví dụ ở trên, chúng ta thấy là `StringProcessor` chỉ có duy nhất một `function process(x)`. Nên mọi đoạn code đều sẽ giống hệt nhau ở việc `implement function` này.

```java
new StringProcessor() {
    @Override
    public String process(String input) {
        // Do something here
        // Chỉ khác nhau đoạn code ở giữa
        return x;
    }
}
```

Thực ra cái chúng ta quan tâm là: `Input -> Process -> Output`. Hãy thử nhìn ở ví dụ dưới cho Lambda Expressions:


```java
// (input) -> input.toUpperCase()
// đầu vào -> đầu ra
System.out.println(getStr("Hello Loda!", input -> input.toUpperCase()));

// Cấu trúc của một lambda như sau:
// parameter -> expression body
```

Trong đó:

- `parameter` là những tham số đầu vào của hàm (một hoặc nhiều)
- `expression body` là phần xử lý `parameter`, bạn cần trả ra đúng kiểu dữ liệu đã khai báo trong `Functional Interface`

Nếu `code` bạn chỉ cần 1 thao tác, thì không cần `return` giống ví dụ ở trên. Còn nếu `code` yêu cầu xử lý nhiều, thì dạng đầy đủ của nó như sau:

```
parameter -> {
    expression body
    [return] // (không trả về nếu là void)
}
```

ví dụ:

```java
System.out.println(getStr("Hello Loda!", input -> {
    String temp =  input + " Oke!!!";
    return temp.toLowerCase();
}));
```

### Functional Interface

Tới đây, bạn đã hiểu ý nghĩa của việc cho ra đời khái niệm `Functional Interface`, nó là một quy định chung phải có để có thể viết code dưới dạng biểu thức `Lambda`. Một số điều cần lưu ý với `Functional Interface` như sau:

#### @FunctionalInterface

`Annotation` này chỉ để bổ sung, nó đánh dấu một `interface` là `Functional Interface`. Lúc này bạn khai báo 2 `abtract function` bên trong `interface` thì sẽ báo lỗi.

```java
@FunctionalInterface // Gắn cái này lên interface, nó đánh dấu interface chỉ được phép có 1 funtion thôi
public interface StringProcessor{
    public String process(String input);
    public String preProcess(String input); // lỗi
}
```

#### default function & static funtion

`Java 8` cải tiến cho phép `interface` được khai báo `code` bên trong nó, với điều kiện `code` phải nằm trong `default` hoặc `static`. `default` và `static` không phá vỡ quy luật của `@FunctionInterfaces`

```java
@FunctionalInterface // Gắn cái này lên interface, nó đánh dấu interface chỉ được phép có 1 funtion thôi
public interface StringProcessor{
    public String process(String input);

    // Mọi class implement StringProcessor đều có thể gọi hàm này để sử dụng luôn
    public default void printf(Object t){
        System.out.println(t);
    }

    // Là hàm static, gọi từ class cũng được.         
    // StringProcessor.concat(a,b)
    public static String concat(String a, String b){
        return a + b;
    }
}
```

### Method reference

Phần này chỉ để bổ sung, không có nó, bạn vẫn có thể sử dụng `Lambda Expressions` bình thường. Nhưng với `Method reference`, code của bạn sẽ còn sạch sẽ hơn nữa.

Ví dụ:

```java
System.out.println(getStr("Hello Loda!", input -> input.toUpperCase()));
// Tương đương với việc viết như này:
System.out.println(getStr("Hello Loda!", String::toUpperCase));
```

hoặc


```java
System.out.println(getStr("Hello Loda!", input -> new String(input));
// Tương đương với việc viết như này:
System.out.println(getStr("Hello Loda!", String::new));
```

`Method reference` là cách viết ngắn gọn, sẽ bỏ qua luôn cả phần `parameter` vì bản thân tên hàm đã biết nó sẽ nhận vào gì và trả ra cái gì rồi. Việc còn lại để `Compiler` lo thôi kakaka. Có các cách để gọi `Method reference` như sau:

- `[Tên Class]::[Tên method]`: Giống với ví dụ ở trên `String::toUpperCase`.
- `[Tên Class]::new`: Tạo ra một đối tượng mới, từ tham số được truyền vào
# Hướng dẫn Stream API

### Khái quát

`Stream` là một abtract layer cho phép bạn xử lý một dòng dữ liệu dựa trên các thao tác đã định nghĩa trước. Bạn có thể tạo `Stream` từ các nguồn dữ liệu như `Collections`, `Arrays` hoặc `I/O resources`. Mặc định các lớp kế thừa của `Collection` đều có hàm `.stream()`:

```java
Collection<String> collection = Arrays.asList("hello", "loda", "kaka");
Stream<String> streamOfCollection = collection.stream(); // Tạo ra một stream từ collection
List<String> list = new ArrayList<>();
Stream<String> stream = list.stream(); // tạo ra 1 luồng
Stream<String> parallelStream = list.parallelStream(); // luồng dữ liệu song song (xử lý trên nhiều thread cùng lúc)
```

### Cách sử dụng

Chức năng của `Stream` là cực kì đa dạng giúp bạn thao tác dữ liệu dễ dàng hơn.

#### `forEach()`: Duyệt qua toàn bộ dữ liệu của bạn
```java
list.stream().forEach(s -> System.out.println(s));
```

#### `map()`: Tạo ra các giá trị mới từ dữ liệu hiện có

```java
Arrays.asList(3, 5, 7)
    .stream() // tạo ra Stream từ List<Integer>
    .map(i -> "loda-"+i) // biến đổi từng phần tử thành String
    .map(String::toUpperCase) // biến đổi từng phần tử thành Upper case
    .forEach(System.out::println); // in ra xem thử
```

#### `filter()` gíup chúng ta thao tác với những dữ liệu mong muốn.

```java
Arrays.asList(2, 3, 5, 7)
    .stream()
    .filter(i -> i % 2 != 0) //từ đây trở đi, chúng ta chỉ muốn làm việc với số lẻ
    .map(i -> "loda-" + i)
    .map(String::toUpperCase)
    .forEach(System.out::println);
```

#### `limit()`: Giới hạn số lượng dữ liệu cần xử lý

```java
IntStream.range(1, 1000).boxed() // Tạo ra Stream có dữ liệu từ 1->999
            .filter(i -> i % 2 != 0)
            .map(i -> "loda-" + i)
            .map(String::toUpperCase)
            .limit(10) // Chúng ta giới hạn lấy 10 cái rồi in ra
            .forEach(System.out::println);
```

#### `sorted()`: sắp xếp `Stream`. Bạn có thể tự định nghĩa cách sort bằng cách thêm Comparator vào

```java
sorted((o1, o2) -> o1.compareTo(o2))
```

```java
List<String> result = IntStream.range(1, 1000).boxed()
                                .filter(i -> i % 2 != 0)
                                .map(i -> "loda-" + i)
                                .map(String::toUpperCase)
                                .limit(10)
                                .sorted(Comparator.naturalOrder()) // một cách khác để sort
                                .collect(Collectors.toList());
```

#### `collect()` giúp chúng ta lấy toàn bộ dữ liệu đã biến đổi trong `Stream` thành đối tượng mình mong muốn.


```java
List<String> result = Stream.of("bạn", "hãy", "like", "Fanpage", "loda","dể","cập","nhật","nhiều","hơn")
                            .filter(s -> {
                                System.out.println("[filtering] " + s);
                                return s.length()>=4;
                            })
                            .map(s -> {
                                System.out.println("[mapping] " + s);
                                return s.toUpperCase();
                            })
                            .limit(3)
                            .collect(Collectors.toList());
System.out.println("----------------------");
System.out.println("Result:");
result.forEach(System.out::println);
```

```makefile
[filtering] bạn // không thoả mãn
[filtering] hãy // tiếp tục tìm, cũng k thoả mãn
[filtering] like // thoả mãn
[mapping] like // mapping nó luôn
[filtering] Fanpage // lại quay lại filter tìm tiếp, thoả mãn
[mapping] Fanpage // mapping
[filtering] loda // thoả mãn
[mapping] loda // mapping
// Đủ 3 trường hợp thoả mãn, dừng.
----------------------
Result:
LIKE
FANPAGE
LODA
```

### Bản chất của Stream

(Có ví dụ trong bài gốc nữa)

`Stream` là `Lazy evaluation`. Hiểu đơn giản là nó sẽ không xử lý dữ liệu trực tiếp qua từng bước, mà chờ bạn khai báo xong tất cả các thao tác `operation` như `map`, `filter`,v.v.. cho tới khi gặp lệnh `.collect()` thì nó thực hiện toàn bộ trong một vòng lặp duy nhất.

Hàm `.collect()` và một số hàm như `min()`, `max()`, `count()` được gọi là `terminal operation`. Khi gọi những function có dạng `terminal` thì `Stream` mới chính thức hoạt động. 

Một lưu ý khi sử dụng là Stream không được tái sử dụng. Vì `Stream` được tạo ra để xử lý dữ liệu chứ không phải để lưu trữ! Nên muốn sử dụng, mỗi lần bạn sẽ cần tạo ra 1 `Stream` mới.


```java
Stream<String> stream =
  Stream.of("loda", ".", "me","like").filter(element -> element.contains("e"));
Optional<String> anyElement = stream.findAny(); //Lấy ra một phần tử bất kỳ trong Stream, nó sẽ trả ra Optional

// Thực hiện dòng lệnh tiếp theo sẽ bắn ra IllegalStateException
Optional<String> firstElement = stream.findFirst();
```

# Khái niệm ThreadPool và Executor trong Java

Một ví dụ đơn giản nhé (Trong thực tế sẽ khác, hãy coi đây là ví dụ nha):

Bây giờ, giả sử bạn có một Server Web. Nếu chúng ta nhận 1 request từ client, chúng ta sẽ xử lý mất 0.5s và trả về kết quả cho người dùng.

Thế nếu có 2 người request cùng lúc? => giải quyết bằng cách mỗi một request sẽ xử lý ở 1 thread, đơn giản.

Thế nếu có 100 người request cùng lúc? => mỗi người tạo một thre... wait a minute.... (nếu 1 tháng có 10M lượt request => tạo ra 10M thread)

Nếu bạn tạo 1-2 thread mới, chả ai trách gì bạn cả. Nhưng nếu bạn tạo liên tục và tới hàng trăm cái mới mỗi lần nhưng lại giải quyết cùng 1 vấn đề thì có lỗ hổng đấy. Vì chi phí của việc tạo 1 thread là tương đối lớn, thường dẫn tới các vấn đề về hiệu năng và cấp phát dữ liệu.

Với việc xử lý các tác vụ liên tục như vậy, có một giải pháp là sử dụng `Thread Pool`.

Ở ví dụ trên, Bây giờ tôi sẽ chỉ sử dụng 30 thread thôi! Và đặt 30 thread này ở trạng thái không làm gì và vứt vào 1 cái `Pool` (1 cái bể chứa, kiểu vậy). Với mỗi request đến, tôi sẽ lấy trong `Pool` ra 1 thread và xử lý công việc, xử lý xong, thì cất thread vào ngược trở lại pool. Đơn giản vậy thôi, như thế chúng ta sẽ không phải tạo mới Thread nữa. Tránh tình tốn chi phí và hiệu năng.

Vấn đề là giả sử có hơn 31 request tới cùng lúc thì sao? rất đúng, trường hợp này là chắc chắn có. Lúc này `Pool` sẽ không còn thread nào sẵn có nữa. Nên 1 request còn lại sẽ bị đẩy vào 1 hàng đợi `BlockingQueue`. Nó sẽ đợi ở đó, bao giờ `Pool` có 1 thread rảnh rỗi thì sẽ quay lại xử lý nốt.

### Cách tạo ThreadPool trong Java

Java Concurrency API hỗ trợ một vài loại `ThreadPool` sau:

- **Cached thread pool**: Mỗi nhiệm vụ sẽ tạo ra thread mới nếu cần, nhưng sẽ tái sử dụng lại các thread cũ. (Cái này vẫn nguy hiểm nhé, nên áp dụng với các task nhỏ, tốn ít tính toán)
- **Fixed thread pool**: giới hạn số lượng tối đa của các Thread được tạo ra. Các task khác đến sau phải chờ trong hàng đợi (BlockingQueue). (Ví dụ đầu bài)
- **Single-threaded pool**: chỉ giữ một Thread thực thi một nhiệm vụ một lúc.
- **Fork/Join pool**: một Thread đặc biệt sử dụng Fork/ Join Framework bằng cách tự động chia nhỏ công việc tính toán cho các core xử lý. (Tính toán song song)

### **Executor**

`Executor` là một class đi kèm trong gói `java.util.concurrent`, là một đối tượng chịu trách nhiệm quản lý các luồng và thực hiện các tác vụ Runnable được yêu cầu xử lý. Nó tách riêng các chi tiết của việc tạo Thread, lập kế hoạch (scheduling), … để chúng ta có thể tập trung phát triển logic của tác vụ mà không quan tâm đến các chi tiết quản lý Thread.


Nói chung nó là thằng wrapper các các bước mình nói ở trên, và quản lý hộ chúng ta.

Chúng có thể tạo một Executor bằng cách sử dụng một trong các phương thức được cung cấp bởi lớp tiện ích `Executors` như sau:

- **newSingleThreadExecutor()**: trong ThreadPool chỉ có 1 Thread và các task (nhiệm vụ) sẽ được xử lý một cách tuần tự.
- **newCachedThreadPool()**: như giải thích ở trên, nó sẽ có 1 số lượng nhất định thread để sử dụng lại, nhưng vẫn sẽ tạo mới thread nếu cần. Mặc định nếu một Thread không được sử dụng trong vòng 60 giây thì Thread đó sẽ bị tắt.
- **newFixedThreadPool(int n)**: trong Pool chỉ có n Thread để xử lý nhiệm vụ, các yêu cầu tới sau bị đẩy vào hàng đợi
- **newScheduledThreadPool(int corePoolSize)**: tương tự như `newCachedThreadPool()` nhưng sẽ có thời gian delay giữa các Thread.
- **newSingleThreadScheduledExecutor()**: tương tự như `newSingleThreadExecutor()` nhưng sẽ có khoảng thời gian delay giữa các Thread.

### **Code chạy thử**

Chúng ta sẽ lấy ví dụ đầu bài để code luôn nhé.

Tạo một class implement `Runnable` để xử lý request đến. (phân biệt `Runnable` và `Thread` nhé các bạn)

```java
public class RequestHandler implements Runnable {
    String name;
    public RequestHandler(String name){
        this.name = name;
    }

    @Override
    public void run() {
        try {
            // Bắt đầu xử lý request đến
            System.out.println(Thread.currentThread().getName() + " Starting process " + name);
            // cho ngủ 500 milis để ví dụ là quá trình xử lý mất 0,5 s
            Thread.sleep(500);
            // Kết thúc xử lý request
            System.out.println(Thread.currentThread().getName() + " Finished process " + name);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
```

### **newSingleThreadExecutor**

```java
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

public class SingleThreadPoolExample {
    public static void main(String[] args) {
        ExecutorService executor = Executors.newSingleThreadExecutor();

        // Có 100 request tới cùng lúc
        for (int i = 0; i < 100; i++) {
            executor.execute(new RequestHandler("request-" + i));
        }
        executor.shutdown(); // Không cho threadpool nhận thêm nhiệm vụ nào nữa

        while (!executor.isTerminated()) {
            // Chờ xử lý hết các request còn chờ trong Queue ...
        }
    }
}
// OUTPUT:
/*
..
..
pool-1-thread-1 Starting process request-98
pool-1-thread-1 Finished process request-98
pool-1-thread-1 Starting process request-99
pool-1-thread-1 Finished process request-99
*/
```

Cả chương trình chỉ có 1 pool, 1 thread duy nhất, xử lý toàn bộ request đến. Cái nào đến sau thì đợi thôi.

### **newFixedThreadPool()**

```java
public class FixedThreadPoolExample {
    public static void main(String[] args) throws InterruptedException {
        ExecutorService executor = Executors.newFixedThreadPool(5);

        // Có 100 request tới cùng lúc

        for (int i = 0; i < 100; i++) {
            executor.execute(new RequestHandler("request-" + i));
        }
        executor.shutdown(); // Không cho threadpool nhận thêm nhiệm vụ nào nữa

        while (!executor.isTerminated()) {
            // Chờ xử lý hết các request còn chờ trong Queue ...
        }
    }
}
// OUTPUT:
/*
..
..
pool-1-thread-2 Finished process request-96
pool-1-thread-5 Starting process request-99
pool-1-thread-3 Finished process request-97
pool-1-thread-4 Finished process request-98
pool-1-thread-5 Finished process request-99
*/
```

Loại này thì chúng ta cố định 5 thread, và nó cử mặc định như vậy mà xài thôi, thiếu thread thì phải chờ tới khi có

### **newCachedThreadPool()**

```java
public class CachedThreadPoolExample {
    public static void main(String[] args) throws InterruptedException {
        ExecutorService executor = Executors.newCachedThreadPool();

        // Có 100 request tới cùng lúc

        for (int i = 0; i < 100; i++) {
            executor.execute(new RequestHandler("request-" + i));
            Thread.sleep(200);
        }
        executor.shutdown(); // Không cho threadpool nhận thêm nhiệm vụ nào nữa

        while (!executor.isTerminated()) {
            // Chờ xử lý hết các request còn chờ trong Queue ...
        }
    }
}

//OUTPUT:
/*
..
..
pool-1-thread-3 Starting process request-98
pool-1-thread-1 Finished process request-96
pool-1-thread-1 Starting process request-99
pool-1-thread-2 Finished process request-97
pool-1-thread-3 Finished process request-98
pool-1-thread-1 Finished process request-99
*/
```

Có chút khởi sắc, chương trình chạy nhanh hơn hẳn. Vì nó được tạo số thread thoải mái nếu cần :)))) Rất nguy hiểm. Nhưng bạn sẽ thấy là có chỗ nó sử dụng lại các thread đã xong trước đó.# ThreadPoolExecutor và nguyên tắc quản lý pool size


- Khái niệm
- Nguyên tắc vận hành
- Code ví dụ

### **Giới thiệu**

`ThreadPoolExecutor` là một class nâng cao hơn của các `ThreadPool` cơ bản trong gói java concurrent. Cụ thể các thể loại ThreadPool khác bạn xem ở đây:

1. Khái niệm ThreadPool và Executor trong Java

Đặc điểm của các loại `ThreadPool` thông thường được cung cấp trong `ExecutorService` là không đủ linh động theo tình huống. điển hình là bị fix số lượng thread, hoặc cho phép tạo quá nhiều thread. Nó thực sự chưa phải phương án tối ưu.

`ThreadPoolExecutor` thì khác, một phiên bản nâng cấp hơn, cho phép chúng ta tùy biến số lượng Thread theo kịch bản. Giúp nó thông minh hơn mấy cái kia một chút.

Ngoài ra còn có `ThreadPoolTaskExecutor` do `Spring Framework` cung cấp cũng hoạt động tương tự

### **Khái niệm**

`ThreadPoolExecutor` và `ThreadPoolTaskExecutor` cũng là `Executor` nhưng nó có thêm các tham số như sau:

- `corePoolSize`: Số lượng Thread mặc định trong `Pool`
- `maxPoolSize`: Số lượng tối đa Thread trong `Pool`
- `queueCapacity`: Số lượng tối da của `BlockingQueue`

### **Nguyên tắc vận hành**

Ví dụ với `ThreadPoolExecutor` có:

- `corePoolSize`: 5
- `maxPoolSize`: 15
- `queueCapacity`: 100

1. Khi có request, nó sẽ tạo trong Pool tối đa 5 thread (`corePoolSize`).
2. Khi số lượng thread vượt quá 5 thread. Nó sẽ cho vào hàng đợi.
3. Khi số lượng hàng đợi full 100 (`queueCapacity`). Lúc này mới bắt đầu tạo thêm Thread mới.
4. Số thread mới được tạo tối đa là 15 (`maxPoolSize`).
5. Khi Request vượt quá số lượng 15 thread. Request sẽ bị từ chối!

Với kịch bản như thế này, bạn sẽ luôn tiết kiệm được số lượng thread sử dụng là 5 trong trường hợp bình thường. Nhưng vẫn có thể handle lên tới 15 thread nếu server quá tải.

Điểm chúng ta hay nhầm lẫn là điều kiện để tạo thêm thread đó là khi **hàng đợi phải full**. Đúng vậy, nếu hàng đợi chưa full, thì có nghĩa chúng ta chưa quá tải.

### **Code ví dụ**

Tạo ra một Runnable để xử lý các nhiệm vụ.

```java
public class RequestHandler implements Runnable {
    String name;
    public RequestHandler(String name){
        this.name = name;
    }

    @Override
    public void run() {
        try {
            System.out.println(Thread.currentThread().getName() + " Starting process " + name);
            // Giả sử nhiệm vụ xử lý hết 0.5s
            Thread.sleep(500);
            System.out.println(Thread.currentThread().getName() + " Finished process " + name);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
```

Tạo ra `ThreadPoolExecutor` để xử lý 1000 request tới dồn dập.

```java
public class ThreadPoolExecutorExample {
    public static void main(String[] args) {
        int corePoolSize = 5;
        int maximumPoolSize = 10;
        int queueCapacity = 100;
        ThreadPoolExecutor executor = new ThreadPoolExecutor(corePoolSize, // Số corePoolSize
                                                             maximumPoolSize, // số maximumPoolSize
                                                             10, // thời gian một thread được sống nếu không làm gì
                                                             TimeUnit.SECONDS,
                                                             new ArrayBlockingQueue<>(queueCapacity)); // Blocking queue để cho request đợi
        // 1000 request đến dồn dập, liền 1 phát, không nghỉ
        for (int i = 0; i < 1000; i++) {
            executor.execute(new RequestHandler("request-" + i));
        }
        executor.shutdown(); // Không cho threadpool nhận thêm nhiệm vụ nào nữa

        while (!executor.isTerminated()) {
            // Chờ xử lý hết các request còn chờ trong Queue ...
        }
    }
}

// OUTPUT
/*
..
..
pool-1-thread-3 Finished process request-96
pool-1-thread-5 Finished process request-97
pool-1-thread-4 Finished process request-98
pool-1-thread-8 Finished process request-100
pool-1-thread-2 Finished process request-99
pool-1-thread-6 Finished process request-102
pool-1-thread-7 Finished process request-101
pool-1-thread-9 Finished process request-104
pool-1-thread-10 Finished process request-103
*/
```

Bạn sẽ thấy là chương trình đã phải sử dụng tới 10 thread để xử lý hết 1000 request cùng 1 lúc. Nhớ là cùng 1 lúc nhé các bạn, thế là nhiều rồi đó. Và theo nguyên tắc. Nó đã tận dụng hết `maxPoolSize` rồi. Mà `queue` vẫn full. Nên các request không ở trong `queue` sẽ bị reject. Dẫn tới chỉ sử lý được `104 request` mà thôi.

Bây giờ, vẫn là ví dụ này, nhưng mỗi request cách nhau `50 milliseconds` thì sẽ như nào, dễ thở hơn k? chỉ 0.05s thôi.

```java
for (int i = 0; i < 1000; i++) {
    executor.execute(new RequestHandler("request-" + i));
    Thread.sleep(50);
}
// OUTPUT:
/*
..
..
pool-1-thread-2 Finished process request-993
pool-1-thread-1 Finished process request-994
pool-1-thread-3 Finished process request-995
pool-1-thread-4 Finished process request-996
pool-1-thread-5 Finished process request-997
pool-1-thread-9 Finished process request-998
pool-1-thread-10 Finished process request-999
*/
```

Xử lý gọn gàng, sạch sẽ các bạn ạ. Sức mạnh của `ThreadPoolExecutor` phát huy rõ rệt hơn. Tận dụng được 10 thread và queue vẫn còn chỗ nên rất nhanh, khác biệt trong một hệ thống có thể đc tính bằng `milliseconds` như vậy đó. nếu mỗi request cách nhau `100 milliseconds` thì nó chỉ cần sử dụng 5 thread thôi.

toàn bộ code mình để tại Github: CODE

Chúc các bạn học tập tốt! ohoho

# Giới thiệu Reactive Programming với Reactor

### **Giới thiệu**

Các ứng dụng hiện nay yêu cầu một tốc độ phản hồi cao để nâng cao trải nghiệm người dùng, giúp hệ thống mượt mà, linh hoạt, không bị đóng băng luồng. Các yêu cầu này cũng là kết quả hướng tới khi chúng ta sử dụng mô hình lập trình theo **Reactive Programming**.

Trong bài viết này, chúng ta sẽ cố gắng làm sáng tỏ mô hình lập trình này thông qua một số khái niệm `Synchronous` và `Asynchronous` , `Blocking` và `Non-Blocking` trước.

### **Synchronous và Asynchronous**

`Synchronous` (Xử lý đồng bộ): là xử lý mà chương trình sẽ chạy theo từng bước, nghĩa là thực hiện xong đoạn code trên mới tới đoạn code kế tiếp và sẽ theo thứ tự từ trên xuống dưới, từ trái qua phải. Đây cũng là nguyên tắc cơ bản mà các bạn đã được học.

`Asynchronous` (Xử lý bất đồng bộ): Ngược lại với xử lý đồng bộ, nghĩa là chương trình có thể hoạt động nhảy cóc, function phía dưới có thể hoạt động mà không cần phải chờ function hay một đoạn code nào đó phía trên thực hiện xong. Dưới đây là minh họa cho việc làm việc với dữ liệu đồng bộ và bất đồng bộ :

!image

Như ta thấy nếu các công việc không liên quan đến nhau thì bất đồng bộ giúp ta tiết kiệm thời gian xử lý hơn và mang lại cho người dùng trải nghiệm tốt hơn.

### **Blocking và Non-Blocking**

Chúng ta có thể hiểu một cách đơn giản khi chúng ta muốn dấy một danh sách `Student`.

Lập trình theo mô hình `Blocking` thì phải chờ đợi chương trình thực hiện lấy tất cả `Student` rồi mới thực hiện các thao tác tiếp theo, hay được gọi là bị đóng băng luồng chờ quá trình đóng gói tất cả `Student` hoàn tất. Do đó sẽ dẫn tốn thời gian chờ đợi nếu số lượng danh sách rất lớn.

Lập trình theo mô hình `Non-Blocking` thì hoạt động ngược lại, không cần phải chờ đợi hoàn thiện cả danh sách `Student` mà với mỗi `Student` nào được đưa ra thì thực hiện thao tác luôn với nó. Điều này dẫn tới không bị đóng băng luồng, kể cả số lượng danh sách lớn.

### **Reactive Programming**

Nói một cách ngắn gọn, **Reactive Programming** là mô hình lập trình mà ở đó dữ liệu được truyền tải dưới dạng luồng ( stream). Mô hình này dưa trên nguyên tắc `Asynchronous` và `Non-Blocking` để làm việc với dữ liệu.

Dưới đây là một số khái niệm mà bạn cần phải biết khi làm việc với mô hình này:

**Publisher:** Là nhà cung cấp dữ liệu, hoặc là nơi phát ra nguồn dữ liệu.

**Subscriber:** Lắng nghe **Publisher**, yêu cầu dữ liệu mới. Hay được gọi Là người tiêu thụ dữ liệu.

**Backpressure:** Là khả năng mà **Subscriber** cho phép **Publisher** có thể xử lý bao nhiêu yêu cầu tại thời điểm đó. Bởi vì **Subscriber** chịu trách nhiệm về luồng dữ liệu, không phải **Publisher** vì nó chỉ cung cấp dữ liệu.

**Stream:** Luồng dữ liệu bao gồm các dữ liệu trả về , các lỗi xảy ra và luồng này phải là luồng bất đồng bộ.

Như vậy dữ liệu của chúng ra sẽ được chuyển thành một dòng (data stream) do đó tránh được việc bị blocking và các dữ liệu phát ra thì đều được subcriber lắng nghe dẫn đến quá trình xử lý và báo lỗi diễn ra một cách đơn giản hơn.

### **Reactor**

**Reactor** là một nền tảng để ta triển khai việc lập trình theo phong cách **reactive programming**. Nó được tích hợp trực tiếp với Java 8 funcion APIs như `CompletableFuture`, `Stream`, `Duration`.

**Reactor** cung cấp 2 loại về **Publisher** :

`Flux`: là một steam phát ra từ 0...n phần tử.

!image

`Mono`: là một steam phát ra từ 0...1 phần tử.

!image

Vậy là các bạn có thể hiểu được **Reactive Programming** phải không nào :D. Các bài viết tới chúng ta sẽ đi sâu hơn về các thực thi cũng như các `function` mà **Reactor** hỗ trợ. Hãy chú ý theo dõi và đừng quên nhận xét để chúng tôi có thể cải thiện các bài viết tốt hơn.

# Giới thiệu Reactor Core


- Maven Dependencies
- Tạo ra một luồng dữ liệu
- Subscribe()
- So sánh với Streams Java 8
- Backpressure
- Concurrency
- Kết luận

### **Tổng Quan**

**Reactor Core** là một thự viện Java 8 implement mô hình **Reactive Programming**. Nó được xây dựng dựa trên **Reactive Streams Specification** \- một tiêu chuẩn để xây dựng ứng dụng `Reactive`.

Trong bài viết này, chúng ta sẽ đi từng bước nhỏ thông qua `Reactor` cho đến khi có cái nhìn toàn cảnh cũng như cách thực thi của **Reactor core**.

### **Maven Dependencies**

Đây là thư viện của `Reactor`, chúng ta có thể lấy thư viện mới nhất tại đây

```
<dependency><groupId>io.projectreactor</groupId>
    <artifactId>reactor-core</artifactId>
    <version>3.2.8.RELEASE</version>
</dependency>
```

### **Tạo ra một luồng dữ liệu**

Để có một ứng dựng phản ứng (reactive), điều đầu tiên chúng ta cần phải làm là tạo ra một luồng dữ liệu. Không có dữ liệu này chúng ta sẽ không có bất cứ điều gì để phản ứng, đó là lý do tại sao đây là bước đầu tiên.

`Reactor core` cung cấp 2 loại dữ liệu cho phép chúng ta thực hiện điều này.

**Flux**

Cách đầu tiên đó là dùng `Flux`. `Flux` là một luồng có thể phát ra **0..n** phần tử. Ví dụ tạo đơn giản:

```java
Flux<Integer> just = Flux.just(1,2,3,4);
```

**Mono**

Cách thứ hai đó là `Mono`. `Mono` là một luồng có thể phát ra **0..1** phần tử. Nó hoạt động gần giống hệ như `Flux`, chỉ là bị giới hạn không quá một phần tử. Ví dụ:

```java
Mono<String> just = Mono.just("atomPtit");
```

Điều lưu ý rằng cả `Flux` và `Mono` đề được triển khai từ interface `Publisher`. Cả hai đều tuần thủ tiêu chuẩn `Reactive`, chúng ta có thể sử dụng interface như sau:

```java
Publisher<String> just = Mono.just("foo");
```

### **Subscribe()**

Hãy luôn ghi nhớ rằng: **Không có gì xảy ra cho đến khi subscribe()** .

Trong `reactor`, khi bạn viết một `Publisher`, dữ liệu không bắt đầu được bơm vào theo mặc định. Thay vào đó, bạn tạo một mô tả trừu tượng về quy định không đồng bộ của bạn(hỗ trợ tái sử dụng).

Để hiểu rõ luồng hoạt động hãy theo dõi qua ví dụ đơn giản sau.

```
<dependency>
    <groupId>io.projectreactor</groupId>
    <artifactId>reactor-core</artifactId>
    <version>3.2.8.RELEASE</version>
</dependency>
<dependency>
    <groupId>ch.qos.logback</groupId>
    <artifactId>logback-classic</artifactId>
    <version>1.1.3</version>
</dependency>
```

Chúng ta thêm thư viện `logback`. Điều này sẽ giúp chúng ta ghi nhật ký đầu ra của quá trình hoạt động `reactor` từ đó hiểu rõ hơn về luồng dữ liệu.

```java
public class ReactorCode {
    public static void main(String[] args) {
        List<Integer> elements = new ArrayList<>();
        Flux.just(1, 2, 3, 4)
                .log()
                .subscribe(elements::add);
    }
}

// OUTPUT:
/*
23:02:16.996 [main] DEBUG reactor.util.Loggers$LoggerFactory - Using Slf4j logging framework
23:02:17.014 [main] INFO  reactor.Flux.Array.1 - | onSubscribe([Synchronous Fuseable] FluxArray.ArraySubscription)
23:02:17.017 [main] INFO  reactor.Flux.Array.1 - | request(unbounded)
23:02:17.018 [main] INFO  reactor.Flux.Array.1 - | onNext(1)
23:02:17.018 [main] INFO  reactor.Flux.Array.1 - | onNext(2)
23:02:17.018 [main] INFO  reactor.Flux.Array.1 - | onNext(3)
23:02:17.018 [main] INFO  reactor.Flux.Array.1 - | onNext(4)
23:02:17.019 [main] INFO  reactor.Flux.Array.1 - | onComplete()
*/
```

Hãy nhìn vào phần output, mọi thứ đều chạy trên main thread. Bây giờ chugn ta đi xem rõ từng dòng thực thi: 1. `onSubscribe()` \- Điều này được gọi thi chúng ra đăng ký (subscriber()) luồng

1. `request(unbounded)` \- Khi chúng ta gọi đăng ký, thì hàm này được chạy ngầm nhằm ý nghĩa tạo đăng ký. Trong trường hợp này chạy mặc định là unbounded (không giới hạn), nghĩa là nó yêu cầu mọi phần tử có sẵn.
2. `onNext()` \- Hàm này được gọi cho mọi phần tử đơn.
3. `onComplete()` \- Hàm này được gọi sau cùng sau khi nhận được phần tử cuối cùng. Trong thực có thể xảy ra các hàm khác như `onError()`, cái mà có thể được gọi khi xảy ra một exception.

### **So sánh với Streams Java 8**

Có vẻ nhiều người vẫn đang nghĩ sự tương đồng với Stream trong Java 8:

```java
List<Integer> collected = Stream.of(1, 2, 3, 4)
  .collect(toList());
```

Sự khác biết cốt lõi là `Reactive` là một hình **push** (đẩy) , trong khi Stream Java 8 là mô hình **pull** (kéo)

Streams Java 8 là `terminal` \- kéo tất cả dữ liệu và trả về một kết quả. Với `Reactive`, chúng ta có một luồng vô hạn đến từ một nguồi tài nguyên bên ngoài, với nhiều người subscribe(). Chúng ta cũng có thể làm những việc như kết hợp các luồng, tiều tiết luồng và `backpressure`.

### **Backpressure**

Trong ví dụ trên, người đăng ký nói với `Publisher` đẩy từng phần tử một. Điều này có thể trở nên quá tải cho người đăng ký phải tiêu thụ hết tất cả tài nguyên của nó.

**Backpressure** đơn giản chỉ là bảo với `Publisher` gửi cho nó ít dữ liệu hơn để ngăn chặn nó bị quá tải.

Ví dụ dưới đây, chúng ta sẽ yêu cầu chỉ gửi 2 phần từ cùng một lúc bằng cách sử dụng `request ()`:

```java
Flux.just(1, 2, 3, 4)
  .log()
  .subscribe(new Subscriber<Integer>() {
    private Subscription s;
    int onNextAmount;

    @Override
    public void onSubscribe(Subscription s) {
        this.s = s;
        s.request(2);
    }

    @Override
    public void onNext(Integer integer) {
        elements.add(integer);
        onNextAmount++;
        if (onNextAmount % 2 == 0) {
            s.request(2);
        }
    }

    @Override
    public void onError(Throwable t) {}

    @Override
    public void onComplete() {}
});

//OUTPUT
/*
23:31:15.395 [main] INFO  reactor.Flux.Array.1 - | onSubscribe([Synchronous Fuseable] FluxArray.ArraySubscription)
23:31:15.397 [main] INFO  reactor.Flux.Array.1 - | request(2)
23:31:15.397 [main] INFO  reactor.Flux.Array.1 - | onNext(1)
23:31:15.398 [main] INFO  reactor.Flux.Array.1 - | onNext(2)
23:31:15.398 [main] INFO  reactor.Flux.Array.1 - | request(2)
23:31:15.398 [main] INFO  reactor.Flux.Array.1 - | onNext(3)
23:31:15.398 [main] INFO  reactor.Flux.Array.1 - | onNext(4)
23:31:15.398 [main] INFO  reactor.Flux.Array.1 - | request(2)
23:31:15.398 [main] INFO  reactor.Flux.Array.1 - | onComplete()
*/
```

Bây giờ chúng ta nhìn thấy hàm `request()` được gọi trước, tiếp theo đó là 2 hàm `onNext()` thực hiện, sau đó lại là `request()`.

### **Concurrency**

Tất cả các ví dụ trên chúng ta đều đang chạy trên một luồng chính. Tuy nhiên, chúng ta có thể kiểm soát luồng nào mà code của chúng ta chạy nếu chúng ta muốn. Các inteface `Scheduler` cung cấp một sự trừu tượng với `asynchronous`.

```java
public class ReactorCode {
    public static void main(String[] args) {
        ExecutorService service = Executors.newFixedThreadPool(10);
        Flux.just(1, 2, 3, 4)
                .log()
                .subscribeOn(Schedulers.fromExecutorService(service))
                .subscribe();

        Flux.just(5, 6, 7, 8)
                .log()
                .subscribeOn(Schedulers.fromExecutorService(service))
                .subscribe();
    }
}

//OUTPUT
/*
23:48:02.972 [main] DEBUG reactor.util.Loggers$LoggerFactory - Using Slf4j logging framework
23:48:02.996 [pool-1-thread-2] INFO  reactor.Flux.Array.2 - | onSubscribe([Synchronous Fuseable] FluxArray.ArraySubscription)
23:48:02.996 [pool-1-thread-1] INFO  reactor.Flux.Array.1 - | onSubscribe([Synchronous Fuseable] FluxArray.ArraySubscription)
23:48:03.000 [pool-1-thread-2] INFO  reactor.Flux.Array.2 - | request(unbounded)
23:48:03.000 [pool-1-thread-1] INFO  reactor.Flux.Array.1 - | request(unbounded)
23:48:03.001 [pool-1-thread-1] INFO  reactor.Flux.Array.1 - | onNext(1)
23:48:03.001 [pool-1-thread-2] INFO  reactor.Flux.Array.2 - | onNext(5)
23:48:03.001 [pool-1-thread-1] INFO  reactor.Flux.Array.1 - | onNext(2)
23:48:03.001 [pool-1-thread-2] INFO  reactor.Flux.Array.2 - | onNext(6)
23:48:03.001 [pool-1-thread-1] INFO  reactor.Flux.Array.1 - | onNext(3)
23:48:03.001 [pool-1-thread-2] INFO  reactor.Flux.Array.2 - | onNext(7)
23:48:03.001 [pool-1-thread-1] INFO  reactor.Flux.Array.1 - | onNext(4)
23:48:03.001 [pool-1-thread-2] INFO  reactor.Flux.Array.2 - | onNext(8)
23:48:03.002 [pool-1-thread-1] INFO  reactor.Flux.Array.1 - | onComplete()
23:48:03.002 [pool-1-thread-2] INFO  reactor.Flux.Array.2 - | onComplete()
*/
```

Ở đây chúng ta dùng ExecutorService, 2 luồng code thực hiện song song trên 2 thread khác nhau, điều mà đã chứng minh bằng output.

### **Kết luận**

Sau bài viết này, chúng tôi đã có cái nhìn tổng quan về `Reactor Core`. Từ các tạo một `Publisher` , các đăng ký, backpressure cũng như xử lý không đồng bộ. Đây cũng là nền tảng để cho chúng tôi viết cái bài viết khác liên quan về `Reactor Core`.

# Bạn thực sự đã biết khi nào dùng Interface khi nào dùng Abstract?

### Tổng quan

Trong java, chúng ta có `class` `abstract` và một `Interface`, ai cũng biết một class có thể `impements` nhiều `Interface` và chỉ kế thừa được một `class` `abstract`. Nhưng bạn thực sự đã biết khi nào thì ta dùng `Interface`, khi nào dùng `Abstract`. Chưa kể bắt đầu từ Java 8 có sự thay đổi về `Interface` càng làm khó phân biệt giữa hai loại này.

Trong bài viết này chúng tôi sẽ đi so sánh một số tính chất của 2 loại này, sau đó là đưa ra ví dụ đơn giải để các bạn hình dung rõ nhất. Cuối cùng là hiểu khi nào thì dùng chúng.

### Sự khác nhau giữa Interface và Abstract

1. Methods: Class `abstract` có các phương thức abstract và non-abstract. Trong khi `Interface` chỉ có phương thức abstract, từ Java 8, thì Interface có thêm 2 loại phương thức là `default` và `static`.
2. Variables: Class `abstract` có thể có các biến final, non-final, static và non-static. Trong khi `Interface` chỉ có các biến static và final.
3. Implementation: Class `abstract` có thể implement các Interface. Trong khi `Interface` thì không thể implement class abstract.
4. Inheritance: Class `abstract` có thể kế thừa được một class khác. Trong khi `Interface` có thể kế thừa được nhiều Interface khác.
5. Accessibility: các thành viên trong `Interface` kiếu mặc định là `public`. Trong khi class `abstract` thì lại có thể là private, protected,..

**_Nguồn: [https://loda.me](https://loda.me) \- còn nhiều cái hay ho lắm!_**

### Abstract là gì?

Abstract(trừu tượng) nghĩa là một cái gì đó không hoàn toàn cụ thể, nó chỉ là một ý tưởng hoặc ý chính của một cái gì đó mà không có bản triển khai cụ thể. Vì vậy Class abstract chỉ là một cấu trúc hoặc hướng dẫn được tạo cho các class cụ thể khác.

Chúng ta có thể nói rằng một class abstract là linh hồn của một class cụ thể, và rõ ràng một cơ thể (class) không thể có hai linh hồn. Đây cũng là lý do Java không hỗ trợ nhiều kế thừa cho các class abstract.

Hãy nhìn vào class abstract sau:
_Xe.class_

```
public abstract class Xe {
    private String dongCo;
     abstract void khoiDongDongCo();
     abstract void dungDongco();
}

```

Chúng tôi tạo một class abstract `Xe` có thuộc tính là `động cơ`, và các phương thức khởi động/ dừng động cơ. `Xe` là một cái gì đó không cụ thể, nó có thể là ô tô, xe máy, ... và rõ ràng không có `Xe` nào mà không tồn tại động cơ và cơ chế khởi động/dừng động cơ cả.

_Oto.class_

```
public class Oto extends Xe {
    @Override
    void khoiDongDongCo() {
        System.out.println("Khởi động động cơ của ôtô");
    }

    @Override
    void dungDongco() {
        System.out.println("Dừng động cơ của ôtô");
    }
}

```

### Interface là gì?

Interface (Giao diện) là một hình thức, giống như một hợp đồng, nó không thể tự làm bất cứ điều gì. Nhưng khi có một class ký kết hợp đồng (implement Interface) này, thì class đó phải tuân theo hợp đồng này.

Trong Interface, chúng tôi định nghĩa các hành vi của một class sẽ thực hiện. Một class có thể có một số cách hành vi khác nhau, cũng giống như nó có thể ký kết được với nhiều hợp đồng khác nhau. Đó cũng là lý do tại sao Java cho phép implement nhiều Interface.

Tiếp nối ví dụ trên, `Xe` có thể di chuyển, vì vậy chúng tôi tạo một Interface Hành động di chuyển và class `Oto` implement nó.

_HanhDongDiChuyen.class_

```
public interface HanhDongDiChuyen {
    void diChuyen();
}

```

Đây là những hành vi của `Oto`, chứ không thuộc tính sẵn có của nó: **Ôtô là xe hơi, ngay cả khi nó không thể di chuyển được!**

_Oto.class_

```
public class Oto extends Xe implements HanhDongDiChuyen{
    @Override
    void khoiDongDongCo() {
        System.out.println("Khởi động động cơ của ôtô");
    }

    @Override
    void dungDongco() {
        System.out.println("Dừng động cơ của ôtô");
    }

    @Override
    public void diChuyen() {
        System.out.println("Ôtô đang di chuyển");
    }
}

```

### Khi nào nên dùng?

1. Class abstract đại diện cho mối quan hệ "IS - A" (Ôtô là Xe)
2. Interface đại diện cho mối quan hệ "like - A" (Ô tô có thể chuyển động).
3. Tạo một class abstract khi bạn đang cung cấp các hướng dẫn cho một class cụ thể.
4. Tạo Interface khi chúng ta cung cấp các hành vi bổ sung cho class cụ thể và những hành vì này không bắt buộc đối với clas đó.

### Kết luận

Mục đích của bài viết này là để giúp bạn hiểu và nắm vững class abstract, Interface và kịch bản sử dụng. Thông qua nổ lực toàn bộ bài viết của chúng tôi, chúng tôi tin chắc rằng bạn đã hiểu được điều gì đó. Cuối cùng, cảm ơn bạn đã đọc bài viết.




# Java Concurrency (Phần 1): Thread

Source: [Java Concurrency (Phần 1): Thread](https://viblo.asia/p/java-concurrency-phan-1-thread-GAWVpevY405)

## 1\. Giới thiệu

Lập trình đồng thời (concurrency) trong Java đề cập đến khả năng của một chương trình Java thực thi nhiều tác vụ đồng thời hoặc song song, tận dụng tối đa các bộ xử lý (CPU) đa lõi (core) hiện đại. Khi các ứng dụng ngày càng trở nên phức tạp và đòi hỏi hiệu suất cao hơn, lập trình đồng thời trở thành yếu tố thiết yếu để cải thiện hiệu năng, khả năng phản hồi và khả năng mở rộng.
Java cung cấp một bộ công cụ và các thư viện phong phú giúp các nhà phát triển tạo ra các ứng dụng đồng thời, quản lý nhiều luồng (threads) và điều phối các tác vụ một cách hiệu quả. Trong bài viết này, chúng sẽ khám phá các khái niệm cơ bản về lập trình đồng thời trong Java.

![image.png](https://images.viblo.asia/full/dab66ffc-6006-45b9-a745-5e5a9f09d9da.png)

## 2\. Định nghĩa Thread

Một thread là một đơn vị thực thi nhỏ hơn một process. **Một process có thể tạo ra nhiều thread trong quá trình thực thi**. Tất cả các thread trong cùng một process sẽ chia sẻ, **dùng chung một số vùng nhớ với nhau** (heap memory, static variables, metaspace, … phần này mình sẽ chia sẻ cụ thể hơn ở một bài viết khác). Vì vậy, việc giao tiếp giữa các thread khá đơn giản và dễ dàng hơn so với giao tiếp giữa các process. Ngoài ra, việc tạo mới/hủy thread đơn giản và tốn ít công hơn so với việc tạo mới/hủy một process. Vì các lý do này, thread còn được gọi là lightweight process.

![image.png](https://images.viblo.asia/full/cb4fb02e-990b-4290-bdb3-c7b3e9d030a1.png)

## 3\. Cách khởi tạo thread

Đây là một câu hỏi thường hay gặp trong phỏng vấn. Bạn có thể tham khảo hoặc trả lời như sau:
Ta có thể phân loại các cách khởi tạo thread như sau:

### 3.1. Tạo trực tiếp thread

sử dụng `new Thread().start()`.

```
new Thread(() -> resource.counter++).start();

```

### 3.2. Khai báo Thread execution method

#### 3.2.1. Kế thừa class Thread

Đây là một cách phổ biến. Chúng ta tạo ra một class mới kế thừa class Thread và ghi đè method run như sau:

```
public class ExtendsThread extends Thread {
    @Override
    public void run() {
        System.out.println("Do something");
    }

    public static void main(String[] args) {
        new ExtendsThread().start();
    }
}

```

#### 3.2.2. Triển khai interface Runnable

Đây cũng là một cách phổ biến, implement Runnable interface và override method run, như sau:

```
public class ImplementsRunnable implements Runnable {
    @Override
    public void run() {
        System.out.println("Do something");
    }

    public static void main(String[] args) {
        ImplementsRunnable runnable = new ImplementsRunnable();
        new Thread(runnable).start();
    }
}

```

#### 3.2.3. Triển khai interface Callable

Tương tự như method trước, ngoại trừ method này có thể nhận giá trị trả về sau khi Thread được thực thi, như sau:

```
public class ImplementsCallable implements Callable<String> {
    @Override
    public String call() throws Exception {
        System.out.println("Do something");
        return "test";
    }

    public static void main(String[] args) throws Exception {
        ImplementsCallable callable = new ImplementsCallable();
        FutureTask<String> futureTask = new FutureTask<>(callable);
        new Thread(futureTask).start();
        System.out.println(futureTask.get());
    }
}

```

#### 3.2.4. Sử dụng class ẩn danh hoặc biểu thức Lambda

```
public class UseAnonymousClass {
    public static void main(String[] args) {
        new Thread(new Runnable() {
            @Override
            public void run() {
                System.out.println("AnonymousClass");
            }
        }).start();

        new Thread(() ->
                System.out.println("Lambda")
        ).start();
    }
}

```

### 3.3. Tạo gián tiếp thread

#### 3.3.1. Sử dụng thread pool của ExecutorService

```
public class UseExecutorService {
    public static void main(String[] args) {
        ExecutorService poolA = Executors.newFixedThreadPool(2);
        poolA.execute(() -> {
            System.out.println("do something");
        });
}

```

#### 3.3.2. Sử dụng thread pool hoặc Stream song song (parallel stream)

```
public class UseForkJoinPool {
    public static void main(String[] args) {
        ForkJoinPool forkJoinPool = new ForkJoinPool();
        forkJoinPool.execute( () -> {
            System.out.println("Do something");
        });

        List<String> list = Arrays.asList("e1");
        list.parallelStream().forEach(System.out::println);
    }
}

```

#### 3.3.3. Sử dụng CompletableFuture

```
public class UseCompletableFuture {
    public static void main(String[] args) throws InterruptedException {
        CompletableFuture<String> cf = CompletableFuture.supplyAsync(() -> {
            System.out.println("5......");
            return "test";
        });
        Thread.sleep(1000);
    }
}

```

#### 3.3.4. Sử dụng class Timer

```
public class UseTimer {
    public static void main(String[] args) {
        Timer timer = new Timer();

        timer.schedule(new TimerTask() {
            @Override
            public void run() {
                System.out.println("9......");
            }
        }, 0, 1000);
    }
}

```

Java chỉ có một cách để tạo thread một cách trực tiếp, đó là thông qua việc tạo new Thread().start(). Do đó, cho dù sử dụng phương thức nào thì cuối cùng nó cũng phụ thuộc vào new Thread().start(). Các đối tượng Runnable, Callable, … chỉ là phần thân của Thread, tức là tác vụ được cung cấp cho Thread để thực thi.

## 4\. Trạng thái của thread

![thread-status.png](https://images.viblo.asia/full/9e0a1831-55d3-48d9-8f04-21ab8bb96b91.png)

Tại một thời điểm, một thread trong Java chỉ có thể ở một trong sáu trạng thái trong vòng đời của nó:

- `NEW`: Khi đối tượng thread được tạo, nó sẽ chuyển sang trạng thái NEW, chẳng hạn như: Thread t = new MyThread();
- `RUNNABLE`: Trạng thái sẵn sàng để chạy. Ta có thể hiểu, nó sẽ được chia thành 2 trường hợp nhỏ hơn: **đang chạy hoặc đang chờ để chạy**. Ví dụ, khi sau, ta gọi method start(), thread đó có thể chưa chạy được ngay mà phải đợi CPU schedule để chạy.
- `BLOCKED`: Trạng thái bị chặn, thread A đang cố giành khóa (lock) nhưng khoá đang giữa bởi thread B, thread A phải đợi, bị blocked cho đến khi khoá được giải phóng.
- `TIME_WAITING`: Trạng thái chờ có thời gian chờ, có thể tự động quay trở lại trạng thái RUNNABLE sau khoảng thời gian xác định.
- `WAITING`: Trạng thái chờ, biểu thị rằng thread A đang chờ các thread khác thực hiện một số hành động cụ thể, như (notification) thông báo cho thread A hoặc (interruption) ngắt thread A. Khác với TIME\_WAITING, trạng thái WAITING không có thời gian timeout, chỉ được wakeup khi có thông báo từ thread khác.
- `TERMINATED`: Trạng thái kết thúc, biểu thị rằng thread đã hoàn thành công việc hoặc dừng lai do gặp exception.

## 5\. Các method cơ bản của thread

### 5.1. start()

Method start() khởi tạo việc thực thi một thread. Nó gọi phương thức run() được xác định trong class thread hoặc runnable object. Thread sẽ chuyển từ trạng thái NEW sang trạng thái RUNNABLE sau khi method này được gọi.

```
public class Main {
    public static void main(String[] args) {
        Thread myThread = new Thread(new MyRunnable());
        myThread.start();
    }
}

```

### 5.2. run()

Method run() chứa mã sẽ được thực thi trong luồng.

```
class MyRunnable implements Runnable {
    public void run() {
        System.out.println("This is a runnable.");
    }
}

```

### 5.3. sleep() và wait()

Method `sleep()` làm cho thread hiện đang thực thi ở chế độ ngủ (TIMED\_WAITING) trong 1 khoảng thời gian được chỉ định (tính bằng milliseconds).

Method wait() khiến thread hiện tại đợi cho đến khi một thread khác gọi notify() hoặc notifyAll() trên cùng một object. Thread sẽ chuyển từ trạng thái RUNNABLE sang trạng thái WAITING nếu dùng wait() không truyền thêm thời gian timeout, còn nếu truyền thêm thời gian timeout - wait(timeout) thì thread sẽ ở trạng thái `TIMED_WAITING`.

Sự khác biệt giữa 2 method:

- **Method wait() cần được đặt trong synchronized code, còn sleep() thì không.**
- Method sleep() không giải phóng khóa, trong khi method wait() sẽ giải phóng khóa.
- Method wait() thường được sử dụng cho tương tác/giao tiếp giữa các thread, còn sleep() thường được sử dụng để tạm dừng thực thi.
- Sau khi method wait() được gọi, thread sẽ không tự động thức dậy; cần một luồng khác gọi method notify() hoặc notifyAll() trên cùng một đối tượng để đánh thức luồng đó. Sau khi method sleep() được thực thi, thread sẽ tự động thức dậy (RUNNABLE).
- sleep() là một method static của class Thread, còn wait() là một method của class Object.

### 5.4. notify() và notifyAll()

notify(): đối với tất cả các thread đang chờ object monitor bằng cách sử dụng bất kỳ method wait() nào, method notify() thông báo cho một trong số các thread đó thức dậy. **Việc lựa chọn chính xác thread nào được đánh thức là mẫu nhiên và chúng ta không thể kiểm soát được** thread được đánh thức.
notifyAll(): Phương pháp này chỉ đơn giản đánh thức tất cả các thread đang chờ trên object monitor.
Mình sẽ nói chi tiết hơn về các method này trong bài giao tiếp giữa các threads.

### 5.5. yield()

Method yield() làm cho thread hiện đang thực thi tạm dừng và cho phép các thread khác thực thi.
Mọi người lưu ý, đây chỉ là hint cho scheduler tạm dừng thread, scheduler có thể bỏ qua cái hint này.
Method này có thể dùng để tái hiện bug do race condition. Tuy nhiên, method này hiếm khi được sử dụng và **mình recommend không dùng method này trong production code**.

### 5.6. join()

Method `join()` cho phép một thread chờ đợi một thread khác hoàn thành. Điều này có thể hữu ích khi bạn cần đảm bảo hoàn thành một số nhiệm vụ nhất định trước khi tiếp tục. Khi thread A gọi method `join()` của thread B, thread A sẽ chuyển sang trạng thái chờ ( `RUNNABLE → WAITING`). Nó vẫn ở trạng thái chờ cho đến khi thread B kết thúc.

Giả sử bạn cần thực hiện một số lệnh gọi API đến các endpoints khác nhau lấy dữ liệu đồng thời. Mỗi lệnh gọi API được thực hiện trong một thread riêng biệt và bạn muốn đợi cho đến khi tất cả các thread hoàn thành yêu cầu API của chúng trước khi tổng hợp (aggregate) kết quả.

```
String[] apiEndpoints = {
    "https://api.example.com/data1",
    "https://api.example.com/data2",
    "https://api.example.com/data3"
};

List<Thread> threads = new ArrayList<>();

List<String> results = new ArrayList<>();

for (String endpoint : apiEndpoints) {
    Thread thread = new Thread(() -> {
        String response = makeApiCall(endpoint);
        synchronized (results) {
            results.add(response);
        }
    });
    threads.add(thread);
    thread.start();
}

// Wait for all threads to complete
try {
    for (Thread thread : threads) {
        thread.join();
    }
} catch (InterruptedException e) {
    e.printStackTrace();
}

// Process and aggregate results
results.forEach(response -> System.out.println("API response: " + response));

```

Nếu mọi người thấy bài viết hữu ích thì nhờ mọi người share để nội dung của Ronin được nhiều người biết hơn.

Cám ơn mọi người. 🙏

* * *

🧑‍💻 150+ Ronin Engineers: [https://roninhub.com/](https://roninhub.com/)

✏️ System Design VN: [https://fb.com/groups/systemdesign.vn](https://fb.com/groups/systemdesign.vn)

📚 Tài liệu khác: [https://roninhub.com/tai-lieu](https://roninhub.com/tai-lieu)

🎬 Youtube: [https://youtube.com/@ronin-engineer](https://youtube.com/@ronin-engineer)





# 「Java 8」Optional

`Java 8` ra đời cùng với một class mới tên là `Optional`. Nhiệm vụ của nó là kiểm soát `null` hộ chúng ta.

### **Khái niệm Optional**

`Optional<T>` là một đối tượng `Generic`, nhiệm vụ chính của nó là **bọc** hay **wrapper** lấy một object khác. Nó chỉ chứa được một object duy nhất bên trong.

Việc bạn lấy giá trị của object bây giờ sẽ thông qua `Optional` và nếu object đó `null` cũng không sao, vì thằng `Optional` kiểm soát nó chặt chẽ hơn là `if else`.

Ví dụ bạn có một đối tượng bất kỳ:

Khi chúng ta thực hiện các thao tác, chúng ta có thể kiểm tra như thế này:

Hmmm..... trông thế này thì khác đếch gì `if (str != null)` =))) Nhiều bạn sẽ tự nghĩ. Đúng là như vậy, nếu nó chỉ làm được đến đây, thì thôi.. nghỉ mịa đee huhu :((

Bây giờ mình sẽ giới thiệu từng tính năng lần lượt của `Optional` để bạn thấy nó kì diệu như nào.

### **ifPresent**

`ifPresent` nhận vào một `Consumer`, nó cũng chỉ là `Functional Interface` thôi các bạn. Nhận vào một đối tượng và thao tác trên nó, không return gì cả.

Nếu bạn chưa rõ `Functional Interface` và `Lambda Expression` thì bạn có thể xem ngay đây, dễ hiểu lém:

Functional Interfaces & Lambda Expressions cực dễ hiểu

### **orElse() và orElseGet()**

`orElse()` lấy ra object trong `Optional`. Nếu `null`, trả về giá trị mặc định do bạn quy định

`orElseGet()` Tương tự `orElse()` nhưng trả ra bằng `Supplier interface`

### **map()**

`map()` giúp chúng ta biến đổi đối tượng bên trong `Optional`.

mình sẽ ví dụ bằng code dễ hiểu hơn.

`code` trông sáng sủa hơn nhiều phải không bạn :3

Trong code ở trên sử dụng `Method reference`, khái niệm này mình đã nói chi tiết tại đây:

Hướng dẫn Method Reference và Lambda Expressions

Khái niệm `map()` mình có nói chi tiết tại đây:

Stream Trong Java 8 cực dễ hiểu!

### **filter()**

`filter()` giúp chúng ta kiểm tra giá trị trong `Optional` nếu không thỏa mãn điều kiện, trả về `empty()`

Tới đây mình đã giới thiệu xong với các bạn các tính năng khá hay ho của `Optional`. Ngoài việc giúp chúng ta kiểm soát `NullException` thì còn giúp `code` của chúng ta sáng sủa hơn rất nhiều và thuận tiện hơn trong nhiều trường hợp yêu cầu điều kiện phức tạp

Chúc các bạn học tập thành công. Và chớ quên like và share ủng hộ nhá ahihi :3

```java
String str = null;
// Tạo ra một đối tượng Optional
Optional<String> optional = Optional.ofNullable(str);
// Bây giờ Optional đã wrap lấy cái str.
```

```java
if (optional.isPresent()) {
    System.out.println(opt.get()); // lấy ra cái str mình đã wrapper
}
```

```java
optional.ifPresent(s -> System.out.println(s));
```

```java
String b = optional.orElse("Giá trị mặc định");
```

```java
String b = optional.orElseGet(() -> {
    StringBuilder sb = new StringBuilder();
    // Thao tác phức tạp
    return sb.toString();
});
```

```java
class Outfit{
    public String type;

    public String getType() { return type; }
}

class Girl{
    private Outfit outfit;

    public Outfit getOutfit() { return outfit; }
}

public String getOutfitType(Girl girl){
    return Optional.ofNullable(girl) // Tạo ra Optional wrap lấy girl
        .map(Girl::getOutfit) // nếu girl != null thì lấy outfit ra xem kakaka :3 ngược lại trả ra Optional.empty()
        .map(Outfit::getType) // nếu outfit != null thì lấy ra xem type của nó
        .orElse("Không mặc gì"); // Nếu cuối cùng là Optional.empty() thì trả ra ngoài Không mặc gì.
}
```

```java
public String getOutfitType(Girl girl){
    return Optional.ofNullable(girl) // Tạo ra Optional wrap lấy girl
        .map(Girl::getOutfit)
        .map(Outfit::getType)
        .filter(s -> s.contains("bikini")) // Nó chỉ chấp nhận giá trị bikini, còn lại dù khác null thì vẫn trả ra ngoài là Optiional.empty()
        .orElse("Không mặc gì"); // Nếu cuối cùng là Optional.empty() thì trả ra ngoài "Không mặc gì".
```

