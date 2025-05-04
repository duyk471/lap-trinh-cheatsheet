# @Component và @Autowired

[Đường dẫn bài viết](https://viblo.asia/p/spring-boot-1-huong-dan-atcomponent-va-atautowired-E375zXvJZGW)

Thường thì giờ đã có [Spring Initializr](https://start.spring.io/) rồi nên thêm dự án mới rất dễ!

### Việc chạy ứng dụng Spring Boot

Chương trình chạy ở hàm `main()` và cần thêm annotation `@SpringBootApplication` trên class chính và gọi `SpringApplication.run(App.class, args);` để chạy project (Cho Spring Boot biết `main()` ở chỗ nào). `SpringApplication.run(App.class, args)` chính là câu lệnh để tạo ra _container_. Sau đó nó tìm toàn bộ các _dependency_ trong project của bạn và đưa vào đó.


```java
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.ApplicationContext;

@SpringBootApplication
public class App {
    public static void main(String[] args) {
        // ApplicationContext chứa toàn bộ dependency trong project.
        SpringApplication.run(App.class, args);
    }
}
```

Spring đặt tên cho:

- _container_ là `ApplicationContext`
- _dependency_ là `Bean` (Để biết cái nào là *dependency* thì dùng `@Component`)

### @Component

@Component là một Annotation (chú thích) đánh dấu trên các Class để giúp Spring biết nó là một Bean. 

Trong quá trình dò các *classes*, khi gặp một class được đánh dấu `@Component` thì nó sẽ tạo ra một instance và đưa vào `ApplicationContext` để quản lý.

```java
// ApplicationContext chính là container, chứa toàn bộ các Bean
ApplicationContext context = SpringApplication.run(App.class, args);
// Khi chạy xong, lúc này context sẽ chứa các Bean có đánh
// dấu @Component.
// Lấy Bean ra bằng cách
Outfit outfit = context.getBean(Outfit.class);
// In ra để xem thử nó là gì
System.out.println("Instance: " + outfit);
// xài hàm wear()
outfit.wear();
// Output
// [1] Instance: me.loda.spring.helloworld.Bikini@1e1f6d9d
// [2] Mặc bikini
```

Danh sách các bước đơn giản là:

1. Chạy `SpringApplication.run(App.class, args);`
2. Dò tìm các Classes cùng cấp.
3. Thấy Class được đánh dấu bằng `@Component` thì tạo `new Customer()` chẳng hạn và đưa vào Context (Hay `ApplicationContext`)

### @Autowired

```
@Component
public class TuiXach {

    @Autowired
    Money tien;

    public TuiXach (Money tien) {
        this.tien = tien;
    }
}
```

Tôi đánh dấu thuộc tính `Money` (Tiền) của `TuiXach` (Túi xách) bởi Annotation `@Autowired`. Điều này nói với Spring Boot hãy tự _inject_ (tiêm) một instance của `Money` vào thuộc tính này khi khởi tạo `TuiXach` (Thuộc tính được đánh dấu là `@Autowired` trong Class được đánh dấu là `Component`).


### Singleton

Điều đặc biệt là các `Bean` được quản lý bên trong `ApplicationContext` đều là singleton.

Tất cả những `Bean` được quản lý trong `ApplicationContext` đều chỉ được tạo ra một lần duy nhất và khi có `Class` yêu cầu `@Autowired` thì nó sẽ lấy đối tượng có sẵn trong `ApplicationContext` để _inject_ vào.

Trong trường hợp bạn muốn mỗi lần sử dụng là một instance hoàn toàn mới. Thì hãy đánh dấu `@Component` đó bằng `@Scope("prototype")`

### Inject là cái quái gì vậy?
Đây không phải là phần hướng dẫn của loda, nhưng mình vẫn chưa hiểu *inject* (về cơ bản nghĩa là tiêm) là gì? Thử lấy ví dụ về truyền dịch.

*Truyền nước biển còn gọi là truyền dịch là phương pháp đưa nhỏ giọt muối và các chất điện giải vào cơ thể bằng đường tĩnh mạch khi có chỉ định của bác sĩ.*

Khi đó thành các *giọt muối và các chất điện giải* sẽ được *inject* và trở thành *thuộc tính* trong bạn. Mỗi một Class trong Java cũng như bạn, là một *đối tượng* (dm giải thích ngu quá mà tính lấy ví dụ Peter Park được nhện chích và thành Spider Man thì có phải hay không???)

*Dependency Injection* cho phép "inject" (tiêm) những dependencies vào một đối tượng. Thay vì đối tượng phải tự tạo ra những Dependencies của chính nó. Điều này sẽ giúp giảm khả năng phụ thuộc giữa những thành phần. # @Autowired - @Primary - @Qualifier

Trong bài viết này chúng ta sẽ cùng tìm hiểu cách `@Autowỉed` vận hành và cách sử dụng 2 Annotation `@Primary`, `@Qualifier`.


### Cách inject Bean của Spring

`@Autowired` đánh dấu cho Spring biết rằng sẽ tự động inject bean tương ứng vào vị trí được đánh dấu.

```
@Component
public class Girl {
    // Đánh dấu để Spring inject một đối tượng Outfit vào đây
    @Autowired
    Outfit outfit;

    // public Girl(Outfit outfit) {
    //     this.outfit = outfit;
    // }

    // GET
    // SET
}
```

Sau khi tìm thấy một class đánh dấu `@Component`. thì quá trình inject `Bean` xảy ra theo cách như sau:

1. Nếu `Class` không có hàm Constructor hay Setter. Thì sẽ sử dụng Java Reflection để đưa đối tượng vào thuộc tính có đánh dấu `@Autowired`.
2. Nếu có hàm Constructor thì sẽ inject Bean vào bởi tham số của hàm
3. Nếu có hàm Setter thì sẽ inject Bean vào bởi tham số của hàm

Như ví dụ ở trên tôi đã sử dụng cách Java Reflection để inject `Bean` vào class `Girl`. 

Bạn cũng có thể gắn `@Autowired` lên trên method, thay vì thuộc tính, chức năng cũng vẫn tương tự, nó sẽ tìm Bean phù hợp với method đó và truyền vào.

```java
@Component
public class Girl {

    // Đánh dấu để Spring inject một đối tượng Outfit vào đây
    @Autowired
    Outfit outfit;

    // Spring sẽ inject outfit thông qua Constructor trước
    public Girl() { }

    // Nếu không tìm thấy Constructor thoả mãn, nó sẽ thông qua setter
    public void setOutfit(Outfit outfit) {
        this.outfit = outfit;
    }

    @Autowired
    // Nếu không tìm thấy Constructor thoả mãn, nó sẽ thông qua setter, 
    // nếu v thì đừng gắn Autowired ở properties ở trên nha
    public void setOutfit(Outfit outfit) {
        this.outfit = outfit;
    }

    // GET
    // SET
}
```

Nếu không sử dụng `@Autowired` thì bạn phải có một Constructor thay thế, hoặc một Setter tương ứng. Khá nhiều các đồng chí gợi ý không dùng `@Autowired` và thay thế bằng:

```java
@Component
public class Calculator {
   private Multiplier multiplier;
   // Cái lày là constructor
   public Calculator(Multiplier multiplier) {
      this.multiplier = multiplier;
   }
}
```

### Vấn đề của @Autowired

Trong thực tế, sẽ có trường hợp chúng ta sử dụng `@Autowired` khi **Spring Boot** có chứa 2 Bean cùng loại trong Context. Lúc này thì **Spring** sẽ bối rối và không biết sử dụng Bean nào để inject vào đối tượng.

### **@Primary**

Cách giải quyết thứ nhất là sử dụng Annotation `@Primary`. `@Primary` là annotation đánh dấu trên một Bean, giúp nó **luôn được ưu tiên lựa** chọn trong trường hợp có nhiều Bean cùng loại trong Context.

Trong ví dụ ở trên, nếu chúng ta để `Suit` là primary. Thì chương trình sẽ chạy bình thường. Và hiển nhiên `Outfit` bên trong `Girl` sẽ là `Suit`.


Chạy thử chương trình:

```java
// ===========================================================================
@Component
@Primary
public class Suit implements Outfit {
    @Override
    public void wear() {
        System.out.println("Đang mặc âu phục");
    }
}
// ===========================================================================
@SpringBootApplication
public class App {
    public static void main(String[] args) {
        // ApplicationContext chính là container, chứa toàn bộ các Bean
        ApplicationContext context = SpringApplication.run(App.class, args);
        // Khi chạy xong, lúc này context sẽ chứa các Bean có đánh
        // dấu @Component.
        Girl girl = context.getBean(Girl.class);
        System.out.println("Girl Instance: " + girl);
        System.out.println("Girl Outfit: " + girl.outfit);
        girl.outfit.wear();
    }
}
// ===========================================================================
// Output:
// Girl Instance: me.loda.spring.helloprimaryqualifier.Girl@eb9a089
// Girl Outfit: me.loda.spring.helloprimaryqualifier.Naked@1688653c
// Đang mặc âu phục
```


### **@Qualifier**

Cách thứ hai, là sử dụng Annotation `@Qualifier`. `@Qualifier` xác định tên của một Bean mà bạn muốn chỉ định inject.

Ví dụ:

```java
@Component("suit")
public class Suit implements Outfit {
    @Override
    public void wear() {
        System.out.println("Mặc âu phục");
    }
}

// ==============================================
@Component
public class Girl {

    Outfit outfit;

    // Đánh dấu để Spring inject một đối tượng Outfit vào đây
    public Girl(@Qualifier("suit") Outfit outfit) {
        // Spring sẽ inject outfit thông qua Constructor đầu tiên
        // Ngoài ra, nó sẽ tìm Bean có @Qualifier("naked") trong context để ịnject
        this.outfit = outfit;
    }
    // GET
    // SET
}
```# Spring Bean Life Cycle + @PostConstruct và @PreDestroy

Tìm hiểu về vòng đời của Bean

### @PostConstruct

`@PostConstruct` được đánh dấu trên một method duy nhất bên trong `Bean`. 

`IoC Container` hoặc `ApplicationContext` sẽ gọi hàm này sau khi một `Bean` được tạo ra và quản lý.

```
@Component
public class Girl {
    
    @PostConstruct
    public void postConstruct(){
        System.out.println("\t>> Đối tượng Girl sau khi khởi tạo xong sẽ chạy hàm này");
    }
}
```

### @PreDestroy

`@PreDestroy` được đánh dấu trên một method duy nhất bên trong `Bean`. 

`IoC Container` hoặc `ApplicationContext` sẽ gọi hàm này trước khi một `Bean` bị xóa hoặc không được quản lý nữa.

```
@Component
public class Girl {

    @PreDestroy
    public void preDestroy(){
        System.out.println("\t>> Đối tượng Girl trước khi bị destroy thì chạy hàm này");
    }
}
```

### Bean Life Cycle

Spring Boot từ thời điểm chạy lần đầu tới khi _shutdown_ thì các `Bean` nó quản lý sẽ có một vòng đời được biểu diễn như ảnh dưới đây.

![](https://images.viblo.asia/2380e7ca-0b97-4be3-9054-27430b29e150.jpg)

Nhìn có vẻ loằng ngoằng, trong series căn bản này, bạn có lẽ sẽ chỉ cần hiểu như sau:

1. Khi `IoC Container` (`ApplicationContext`) tìm thấy một Bean cần quản lý, nó sẽ khởi tạo bằng `Constructor`.
2. inject dependencies vào `Bean` bằng Setter, và thực hiện các quá trình cài đặt khác vào `Bean` như `setBeanName`, `setBeanClassLoader`, v.v..
3. Hàm đánh dấu `@PostConstruct` được gọi
4. Tiền xử lý sau khi `@PostConstruct` được gọi.
5. `Bean` sẵn sàng để hoạt động
6. Nếu `IoC Container` không quản lý bean nữa hoặc bị shutdown nó sẽ gọi hàm `@PreDestroy` trong `Bean`
7. Xóa `Bean`.

### Ví dụ

Chúng ta tạo ra class `Girl` bao gồm:

```java
import org.springframework.stereotype.Component;
import javax.annotation.PostConstruct;
import javax.annotation.PreDestroy;

@Component
public class Girl {

    @PostConstruct
    public void postConstruct(){
        System.out.println("\t>> Đối tượng Girl sau khi khởi tạo xong sẽ chạy hàm này");
    }

    @PreDestroy
    public void preDestroy(){
        System.out.println("\t>> Đối tượng Girl trước khi bị destroy thì chạy hàm này");
    }
}
```

In ra màn hình quá Spring Boot chạy lần đầu cho tới khi shutdown:

```java
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.ApplicationContext;
import org.springframework.context.ConfigurableApplicationContext;

@SpringBootApplication
public class App {
    public static void main(String[] args) {
        // ApplicationContext chính là container, chứa toàn bộ các Bean
        System.out.println("> Trước khi IoC Container được khởi tạo");
        ApplicationContext context = SpringApplication.run(App.class, args);
        System.out.println("> Sau khi IoC Container được khởi tạo");
        // Khi chạy xong, lúc này context sẽ chứa các Bean có đánh
        // dấu @Component.
        Girl girl = context.getBean(Girl.class);
        System.out.println("> Trước khi IoC Container destroy Girl");
        ((ConfigurableApplicationContext) context).getBeanFactory().destroyBean(girl);
        System.out.println("> Sau khi IoC Container destroy Girl");

    }
}
```

Output:

```
> Trước khi IoC Container được khởi tạo
> Trước khi IoC Container được khởi tạo
	>> Đối tượng Girl sau khi khởi tạo xong sẽ chạy hàm này
> Sau khi IoC Container được khởi tạo
> Trước khi IoC Container destroy Girl
	>> Đối tượng Girl trước khi bị destroy thì chạy hàm này
> Sau khi IoC Container destroy Girl
```

Bạn sẽ thấy dòng _"Trước khi IoC Container được khởi tạo"_ được chạy 2 lần. Điều này xảy ra bởi vì hàm App.main(args) được chạy 2 lần!

Lần đầu là do chúng ta chạy. Lần thứ hai là do Spring Boot chạy sau khi nó được gọi `SpringApplication.run(App.class, args)`. Đây là lúc mà IoC Container (`ApplicationContext`) được tạo ra và đi tìm `Bean`.

### Ý nghĩa.

`@PostConstruct` và `@PreDestroy` là 2 Annotation cực kỳ ý nghĩa, nếu bạn nắm được vòng đời của một `Bean`, bạn có thể tận dụng nó để làm các nhiệm vụ riêng như setting, thêm giá trị mặc định trong thuộc tính sau khi tạo, xóa dữ liệu trước khi xóa, v.v.. Rất nhiều chức năng khác tùy theo nhu cầu.# @Component vs @Service vs @Repository

### Kiến trúc trong Spring Boot

Kiến trúc MVC trong Spring Boot được xây dựng dựa trên tư tưởng "độc lập" kết hợp với các nguyên lý thiết kế hướng đối tượng (một đại diện tiêu biểu là Dependency Inversion). 

Độc lập ở đây ám chỉ việc các layer phục vụ các mục đích nhất định, khi muốn thực hiện một công việc ngoài phạm vi thì sẽ đưa công việc xuống các layer thấp hơn.

Kiến trúc Controller - Service - Repository chia project thành 3 lớp:

- Consumer Layer hay Controller: là tầng giao tiếp với bên ngoài và handler các request từ bên ngoài tới hệ thống.
- Service Layer: Thực hiện các nghiệp vụ và xử lý logic
- Repository Layer: Chịu trách nhiệm giao tiếp với các DB, thiết bị lưu trữ, xử lý query và trả về các kiểu dữ liệu mà tầng Service yêu cầu.

### @Controller vs @Service vs @Repository

Để phục vụ cho kiến trúc ở trên, Spring Boot tạo ra 3 Annotation là `@Controller`, `@Service`, `@Repository` để chúng ta có thể đánh dấu các tầng với nhau.

- `@Service` Đánh dấu một Class là tầng Service, phục vụ các logic nghiệp vụ.
- `@Repository` Đánh dấu một Class Là tầng Repository, phục vụ truy xuất dữ liệu.

### Implement

Chạy chương trình:

```java
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.ApplicationContext;

@SpringBootApplication
public class App {

    public static void main(String[] args) {
        ApplicationContext context = SpringApplication.run(App.class, args);

        // Lấy ra bean GirlService
        GirlService girlService = context.getBean(GirlService.class);
        // Lấu ra random một cô gái từ tầng service
        Girl girl = girlService.getRandomGirl();
        // In ra màn hình
        System.out.println(girl);

    }
}
```

### Giải thích

Về bản chất `@Service` và `@Repository` cũng chính là `@Component`. Nhưng đặt tên khác nhau để giúp chúng ta phân biệt các tầng với nhau.

Cùng nhìn vào source code của 2 Annotation này:

_Service.java_

```java
@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Component // Cũng là một @Component
public @interface Service {
    @AliasFor(
        annotation = Component.class
    )
    String value() default "";
}
```

_Repository.java_

```java
@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Component
public @interface Repository {
    @AliasFor(
        annotation = Component.class
    )
    String value() default "";
}
```

Trong các bài đầu tiên chúng ta đã biết `@Component` đánh dấu cho Spring Boot biết Class đó là `Bean`. Và hiển nhiên `@Service` và `@Repository` cũng vậy. Vì thế ở ví dụ trên chúng ta có thể lấy `GirlService` từ `ApplicationContext`. Về bản chất thì bạn có thể sử dụng thay thế 3 Annotation `@Component`, `@Service` và `@Repository` cho nhau mà không ảnh hưởng gì tới code của bạn cả. Nó vẫn sẽ hoạt động.

Tuy nhiên từ góc độ thiết kế thì chúng ta cần phân rõ 3 Annotation này cho các Class đảm nhiệm đúng nhiệm vụ của nó.

- `@Service` gắn cho các `Bean` đảm nhiệm xử lý logic
- `@Repository` gắn cho các `Bean` đảm nhiệm giao tiếp với DB
- `@Component` gắn cho các `Bean` khác.

# Component Scan là gì?

Chúng ta sẽ tìm hiểu thêm về cách Spring Boot tìm kiếm `Bean` trong project của bạn như thế nào.


### Component Scan
Trong bài 1 tôi có đề cập một lần về việc Spring Boot khi chạy sẽ dò tìm toàn bộ các _Class cùng cấp_ hoặc ở trong các _package thấp hơn_ và tạo ra Bean từ các Class tìm thấy.

Trong trường hợp bạn muốn tuỳ chỉnh cấu hình cho Spring Boot chỉ tìm kiếm các bean trong một package nhất định thì có các cách sau đây:

1. Sử dụng `@ComponentScan`
2. Sử dụng `scanBasePackages` tromg `@SpringBootApplication`.

Bạn có thể cấu hình cho Spring Boot Tìm kiếm các Bean ở nhiều package khác nhau bằng cách

### Cách 1:`@ComponentScan`

```java
@ComponentScan("me.loda.spring.componentscan.others")
// Tìm nhiều package khác nhau
// @ComponentScan({"me.loda.spring.componentscan.others2","me.loda.spring.componentscan.others"})

@SpringBootApplication
public class App {
    ...
}
```

### Cách 2:`scanBasePackages`

```java
@SpringBootApplication(scanBasePackages = "me.loda.spring.componentscan.others")
// Tìm nhiều package khác nhau
// @SpringBootApplication(scanBasePackages = {"me.loda.spring.componentscan.others", "me.loda.spring.componentscan.others2"})
public class App {
  ...
}
```

Cả 2 cách đều cho kết quả in ra màn hình như sau:

```
Bean Girl không tồn tại
Bean: OtherGirl.java
```

Lúc này, Spring Boot chỉ tìm kiếm các bean trong package `others` mà thôi. Nên khi lấy ra `Girl` thì nó không tồn tại trong `Context`.
# @Configuration và @Bean

### @Configuration và @Bean

`@Configuration` là một Annotation đánh dấu trên một `Class` cho phép Spring Boot biết được đây là nơi định nghĩa ra các _Bean_.

`@Bean` là một Annotation được đánh dấu trên các `method` cho phép Spring Boot biết được đây là _Bean_ và sẽ thực hiện đưa _Bean_ này vào `Context`.

`@Bean` sẽ nằm trong các class có đánh dấu `@Configuration`.

Ví dụ:

_SimpleBean.java_

```java
// Một class cơ bản, không sử dụng `@Component`

public class SimpleBean {
    private String username;

    public SimpleBean(String username) {
        setUsername(username);
    }

    @Override
    public String toString() {
        return "This is a simple bean, name: " + username;
    }
}
```

_AppConfig.java_

```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class AppConfig {
    @Bean
    SimpleBean simpleBeanConfigure(){
        // Khởi tạo một instance của SimpleBean và trả ra ngoài
        return new SimpleBean("loda");
    }
}
```

_App.java_

```java
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.ApplicationContext;

@SpringBootApplication
public class App {
    public static void main(String[] args) {
        ApplicationContext context = SpringApplication.run(App.class, args);
        // Lấy ra bean SimpleBean trong Context
        SimpleBean simpleBean = context.getBean(SimpleBean.class);
        // In ra màn hình
        System.out.println("Simple Bean Example: " + simpleBean.toString());
        // OUTPUT: Simple Bean Example: This is a simple bean, name: loda
    }
}
```

Bạn sẽ thấy là `SimpleBean` là một object được quản lý trong `Context` của Spring Boot, mặc dù trong bài này, chúng ta không hề sử dụng tới các khái niệm `@Component`.

### Hoạt động nền

Đằng sau chương trình, Spring Boot lần đầu khởi chạy, ngoài việc đi tìm các `@Component` thì nó còn làm một nhiệm vụ nữa là tìm các class `@Configuration`.

1. Đi tìm class có đánh dấu `@Configuration`
2. Tạo ra đối tượng từ class có đánh dấu `@Configuration`
3. tìm các method có đánh dấu `@Bean` trong đối tượng vừa tạo
4. Thực hiện gọi các method có đánh dấu `@Bean` để lấy ra các _Bean_ và đưa vào \`Context.

Ngoài ra, về bản chất, `@Configuration` cũng là `@Component`. Nó chỉ khác ở ý nghĩa sử dụng. (Giống với việc class được đánh dấu `@Service` chỉ nên phục vụ logic vậy).

```java
@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Component // Nó được đánh dấu là Component
public @interface Configuration {
    @AliasFor(
        annotation = Component.class
    )
    String value() default "";
}
```

### Có ý nghĩa gì?

Nhiều bạn sẽ tự hỏi rằng `@Configuration` và `@Bean` sẽ có ý nghĩa gì khi chúng ta đã có `@Component`? Sao không đánh dấu `SimpleBean` là `@Component` cho nhanh?

Các bạn thắc mắc rất đúng, và việc sử dụng `@Component` cũng hoàn toàn ổn.

Thông thường thì các class được đánh dấu `@Component` đều có thể tạo tự động và inject tự động được.

Tuy nhiên trong thực tế, nếu một `Bean` có quá nhiều logic để khởi tạo và cấu hình, thì chúng ta sẽ sử dụng `@Configuration` và `@Bean` để tự tay tạo ra `Bean`. Việc tự tay tạo ra `Bean` như này có thể hiểu phần nào là chúng ta đang _config_ cho chương trình.

### Ví dụ

[Đọc trong trang bài viết](https://viblo.asia/p/spring-boot-6-atconfiguration-va-atbean-bJzKmyprK9N#_vi-du-5)

### @Bean có tham số

Nếu method được đánh dấu bởi `@Bean` có tham số truyền vào, thì Spring Boot sẽ tự inject các Bean đã có trong `Context` vào làm tham số.

Ví dụ:

_AppConfig.java_

```java
@Configuration
public class AppConfig {

    @Bean
    SimpleBean simpleBeanConfigure(){
        // Khởi tạo một instance của SimpleBean và trả ra ngoài
        return new SimpleBean("loda");
    }

    @Bean("mysqlConnector")
    DatabaseConnector mysqlConfigure(SimpleBean simpleBean) { // SimpleBean được tự động inject vào.
        DatabaseConnector mySqlConnector = new MySqlConnector();
        mySqlConnector.setUrl("jdbc:mysql://host1:33060/" + simpleBean.getUsername());
        // Set username, password, format, v.v...
        return mySqlConnector;
    }
}
```

### Thực tế

Trong thực tế, việc sử dụng `@Configuration` là hết sức cần thiết, và nó đóng vai trò là nơi cấu hình cho toàn bộ ứng dụng của bạn. Một Ứng dụng sẽ có nhiều class chứa `@Configuration` và mỗi class sẽ đảm nhận cấu hình một bộ phận gì đó trong ứng dụng.
# Application Config và @Value

Trong thực tế không phải lúc nào chúng ta cũng nên để mọi thứ trong code của mình. Có những thông số tốt hơn hết nên được truyền từ bên ngoài vào ứng dụng, để giúp ứng dụng của bạn dễ dàng thay đổi giữa các môi trường khác nhau. Để phục vụ điều này, chúng ta sẽ tìm hiểu về khái niệm config ứng dụng Spring Boot với `application.properties`


### application.properties

Trong Spring Boot, các thông tin cấu hình mặc định được lấy từ file `resources/applications.properties`. Ví dụ, bạn muốn Spring Boot chạy trên port 8081 thay vì 8080 thì `server.port = 8081`

Hoặc bạn muốn log của chương trình chi tiết hơn. Hãy chuyển nó sang dậng Debug bằng cách config như sau: `logging.level.root=DEBUG`

### @Value

Trong trường hợp, bạn muốn tự config những giá trị của riêng mình, thì Spring Boot hỗ trợ bạn với annotation `@Value`

Ví dụ, tôi muốn cấu hình cho thông tin database của tôi từ bên ngoài ứng dụng

_application.properties_

```
loda.mysql.url=jdbc:mysql://host1:33060/loda
```

`@Value` được sử dụng trên thuộc tính của class, Có nhiệm vụ lấy thông tin từ file properties và gán vào biến.

```
public class AppConfig {
    // Lấy giá trị config từ file application.properties
    @Value("${loda.mysql.url}")
    String mysqlUrl;
}
```

Thông tin truyền vào annottaion `@Value` chính là tên của cấu hình đặt trong dấu `${name}`

### Ví dụ và đọc thêm
[Phần ví dụ đầy đủ cho bài viết này](https://viblo.asia/p/spring-boot-7-spring-boot-application-config-va-atvalue-RQqKLwr657z#_vi-du-4)

Sau bài này bạn có thể xem thêm nội dung sau:

1. [Hướng dẫn sử dụng Spring Properties với `@ConfigurationProperties`](https://www.baeldung.com/configuration-properties-in-spring-boot)
2. Hướng dẫn sử dụng Spring Profiles# @Controller + Thymeleaf

### **@Controller**

Trong bài viết số 4, khi nói về `@Service` và `@Repository` tôi đã đề cập tới kiến trúc trong **Spring Boot**.

Để xây dựng một trang web với **Spring Boot**, bạn sẽ cần tuân theo quy trình như hình dưới đây:

![](https://images.viblo.asia/95ff7fc2-b915-419a-bb14-38bfdf5bf250.png)

`@Controller` là nơi tiếp nhận các thông tin request từ phía người dùng. Nó có nhiệm vụ đón nhận các yêu cầu (kèm theo thông tin request) và chuyển các yêu cầu này xuống cho tầng `@Serivce` xử lý logic.

### **HTML**

Để tạo ra một trang Web, bạn sẽ cần tạo ra các trang html để trả về cho người dùng. Mặc định trong **Spring Boot**, các file html này sẽ được lưu trữ trong thư mục `resources/templates`. **Spring Boot + Thymeleaf** sẽ tìm kiếm các file này theo tên. Ví dụ "index" sẽ tương ứng với "index.html".

### Giải thích 1

Bản thân `@Controller` Cũng là một `@Component` nên nó sẽ được **Spring Boot** quản lý.

```java
@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Component
public @interface Controller {
    @AliasFor(
        annotation = Component.class
    )
    String value() default "";
}
```

**Spring Boot** sẽ lắng nghe các request từ phía người dùng. và tùy theo đường dẫn `path` là gì, nó sẽ mapping tới hàm xử lý tương ứng trong `@Controller`.

Như ví dụ trên, tôi sử dụng `GET` vào địa chỉ `localhost:8080/` ( đường dẫn là `/`). **Spring Boot** sẽ gọi tới hàm có gắn `@GetMapping("/")` và yêu cầu hàm này xử lý request này.

### Giải thích 2

Tới đây bạn hãy tham chiếu đường dẫn request với hàm xử lý nó:

```java
// http://localhost:8080/hello?name=Loda
    @GetMapping("/hello")
    public String hello(
            // Request param "name" sẽ được gán giá trị vào biến String
            @RequestParam(name = "name", required = false, defaultValue = "") String name,
            // Model là một object của Spring Boot, được gắn vào trong mọi request.
            Model model
    ) {
        // Gắn vào model giá trị name nhận được
        model.addAttribute("name", name);

        return "hello"; // trả về file hello.html cùng với thông tin trong object Model
    }
```

Khi request lên, chúng ta nhận được giá trị của `name` và tiếp tục gán nó vào `Model`.

![](https://images.viblo.asia/c1490e7d-3093-430e-b22e-32ab1395c199.png)

`Model` ở đây là một object được **Spring Boot** đính kém trong mỗi response. `Model` chứa các thông tin mà bạn muốn trả về và **Template Engine** sẽ trích xuất thông tin này ra thành html và đưa cho người dùng. Trong file `hello.html` tôi lấy giá trị của `name` trong `Model` ra bằng cách sử dụng cú pháp của `Thymeleaf`

```
<h1 th:text="'Hello, ' + ${name}"></h1>
```

# Giải thích cách Thymeleaf vận hành + Expression

### Thymeleaf

Thymeleaf là một Java Template Engine. Có nhiệm vụ xử lý và generate ra các file HTML, XML, v.v.. Các file HMTL do Thymeleaf tạo ra là nhờ kết hợp dữ liệu và template + quy tắc để sinh ra một file HTML chứa đầy đủ thông tin. Việc của bạn là cung cấp dữ liệu và quy định template như nào, còn việc dùng các thông tin đó để render ra HTML sẽ do Thymeleaf giải quyết.

### Cú pháp

Cú pháp của Thymeleaf sẽ là một attributes (Thuộc tính) của thẻ HTML và bắt đầu bằng chữ `th:`.

Ví dụ: Để truyền dữ liệu từ biến `name` trong Java vào một thẻ `H1` của HTML.

```html
<h1 th:text="${name}"></h1>
```

Chúng ta viết thẻ h1 như bình thường, nhưng không chứa bất cứ text nào trong thẻ. Mà sử dụng cú pháp `th:text="${name}"` để Thymeleaf lấy thông tin từ biến `name` và đưa vào thẻ `H1`.

Kết quả khi render ra, thuộc tính `th:text` biến mất và giá trị biến `name` được đưa vào trong thẻ `H1`. Đó là cách Thymeleaf hoạt động:

```java
// Giả sử String name = "loda"
<h1>Loda</h1>
```

### Model & View Trong Spring Boot

`Model` là đối tượng lưu giữ thông tin và được sử dụng bởi Template Engine để generate ra webpage. Có thể hiểu nó là `Context` của Thymeleaf. `Model` lưu giữ thông tin dưới dạng key-value.

Trong template thymeleaf, để lấy các thông tin trong `Model`. bạn sẽ sử dụng `Thymeleaf Standard Expression`.

1. `${...}`: Giá trị của một biến.
2. `*{...}`: Giá trị của một biến được chỉ định.

Ngoài ra, để lấy thông tin đặc biệt hơn:

1. `#{...}`: Lấy message
2. `@{...}`: Lấy đường dẫn URL dựa theo context của server

Nói tới đây có lẽ hơi khó hiểu, chúng ta sẽ dùng ví dụ để hiểu rõ từng loại Expression.

### `${...}` - Variables Expressions

Trên Controller bạn đưa vào một số giá trị:

```
model.addAttribute("today", "Monday");
```

Để lấy giá trị của biến `today`, tôi sử dụng `${...}`

```
<p>Today is: <span th:text="${today}"></span>.</p>
```

### `*{...}` - Variables Expressions on selections

Dấu `*` còn gọi là `asterisk syntax`. Chức năng của nó giống với `${...}` là lấy giá trị của một biến. Điểm khác biệt là nó sẽ lấy ra giá trị của một biến cho trước bởi `th:object`

```html
<div th:object="${session.user}"><!-- th:object tồn tại trong phạm vi của thẻ div này -->
    <!-- Lấy ra tên của đối tượng session.user -->
    <p>Name: <spanth:text="*{firstName}"></span>.</p><!-- Lấy ra lastName của đối tượng session.user -->
    <p>Surname: <spanth:text="*{lastName}"></span>.</p>
</div>
```

Vậy đoạn code ở trên tương đương với:

```
<div><p>Name: <spanth:text="${session.user.firstName}"></span>.</p><p>Surname: <spanth:text="${session.user.lastName}"></span>.</p></div>
```

Còn `${...}` sẽ lấy ra giá trị cục bộ trong `Context` hay `Model`.

### `#{...}` - Message Expression

Ví dụ, trong file config `.properties` của tôi có một message chào người dùng bằng nhiều ngôn ngữ.

```
home.welcome=¡Bienvenido a nuestra tienda de comestibles!
```

Thì cách lấy nó ra nhanh nhất là:

```
<p th:utext="#{home.welcome}">Xin chào các bạn!</p>
```

Đoạn text tiếng việt bên trong thẻ `p` sẽ bị thay thế bởi thymeleaf khi render `#{home.welcome}`.

### `@{...}` - URL Expression

`@{...}` xử lý và trả ra giá trị URL theo context của máy chủ cho chúng ta.

Ví dụ:

```
// <a th:href="@{/order/list}">
// If our app is installed at http://localhost:8080/myapp, this URL will output:
// <a href="/myapp/order/list">
<a href="details.html" th:href="@{/order/details(orderId=${o.id})}">view</a>
<ahref="details.html"th:href="@{/order/{orderId}/details(orderId=${o.id})}">view</a>
```

Nếu bắt dầu bằng dấu `/` thì nó sẽ là Relative URL và sẽ tương ứng theo context của máy chủ của bạn.# @RequestMapping + @PostMapping + @ModelAttribute + @RequestParam

### **@PostMapping**

`@PostMapping` có nhiệm vụ đánh dấu hàm xử lý POST request trong Controller.

Ví dụ:

Tôi có 2 hàm xử lý, một cho `GET` method và một cho `POST`.

Cả hai đều chung đường dẫn nhưng bạn nên biết rằng cùng một `path` nhưng khác `method` thì sẽ xử lý khác nhau.

```
@Controller
public class WebController {
    @GetMapping("/addTodo")
    public String addTodo(Model model) {
        return "addTodo";
    }

    @PostMapping("/addTodo")
    public String addTodo(Model model) {
        return "success";
    }
}
```

### **@RequestMapping**

Trong trường hợp bạn muốn tất cả các method đều dùng chung một cách xử lý thì có thể sử dụng Annotation `@RequestMapping`. @RequestMapping là một annotation có ý nghĩa và mục đích sử dụng rộng hơn các loại @GetMapping, @PostMapping,v.v.. Nếu không chỉ định method cho `@RequestMapping` thì nó sẽ nhận toàn bộ các method.

```java
@Controller
@RequestMapping("api/v1")
public class WebController {

    // Đường dẫn lúc này là: /api/v1/addTodo và method GET
    @RequestMapping(value = "/addTodo", method = RequestMethod.GET)
    public String addTodo(Model model) {
        return "addTodo";
    }

    // Đường dẫn lúc này là: /api/v1/addTodo và method POST
    @RequestMapping(value = "/addTodo", method = RequestMethod.POST)
    public String addTodo(@ModelAttribute Todo todo) {
        return "success";
    }
}
```

### GetMapping

Khi tôi request lên server như này `http://localhost:8080/listTodo?limit=2`.

```java
@GetMapping("/listTodo")
public String index(Model model, @RequestParam(value = "limit", required = false) Integer limit) {
    // Trả về đối tượng todoList.
    // Nếu người dùng gửi lên param limit thì trả về sublist của todoList
    model.addAttribute("todoList", limit != null ? todoList.subList(0, limit) : todoList);

    // Trả về template "listTodo.html"
    return "listTodo";
}
```# Spring Boot JPA + MySQL

### **Spring Boot JPA**

**Spring Boot JPA** là một phần trong hệ sinh thái Spring Data, nó tạo ra một layer ở giữa tầng service và database, giúp chúng ta thao tác với database một cách dễ dàng hơn, tự động config và giảm thiểu code thừa thãi.

**Spring Boot JPA** đã wrapper Hibernate và tạo ra một interface mạnh mẽ. Nếu như bạn gặp khó khăn khi làm việc với Hibernate thì đừng lo, bạn hãy để **Spring JPA** làm hộ.

### **Tạo Table và dữ liệu**

Trước khi bắt đầu, chúng ta cần tạo ra dữ liệu trong Database. Ở đây tôi chọn `MySQL`. Dưới đây là SQL Script để tạo DATABASE `micro_db`. Chứa một TABLE duy nhất là `User`.

### **Tạo Model User**

Khi đã có dữ liệu trong Database. Chúng ta sẽ tạo một Class trong Java để mapping thông tin. Cần biết thêm về Hibernate. Đây là _User.java_:

```java
@Entity
@Table(name = "user")
@Data
public class User implements Serializable {
    private static final long serialVersionUID = -297553281792804396L;
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    // Mapping thông tin biến với tên cột trong Database
    @Column(name = "hp")
    private int hp;
    // Nếu không đánh dấu @Column thì sẽ mapping tự động theo tên biến
    private int atk;
}
```

### **Vấn đề của Hibernate truyền thống**

Thông thường, khi bạn đã định nghĩa `Entity` tương ứng với `Table` trong DB thông qua Hibernate. Thì nhiệm vụ tiếp theo sẽ là tạo ra các class thao tác với DB.

Ví dụ muốn query lấy tất cả `User` bằng Hibernate truyền thống sẽ như sau. Nó sẽ rất dài nên khi nắm được vấn đề này, Spring Data đã wrapper lên Hibernate một lớp nữa gọi là Spring JPA.

### **JpaRepository**

Để sử dụng **Spring JPA**, bạn cần sử dụng interface `JpaRepository`.

Yêu cầu của interface này đó là bạn phải cung cấp 2 thông tin:

1. Entity (Đối tượng tương ứng với Table trong DB)
2. Kiểu dữ liệu của khóa chính (PrimaryKey)

Ví dụ: Tôi muốn lấy thông tin của bảng `User` thì làm như sau:

```
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;

@Repository
public interface UserRepository extends JpaRepository<User, Long> {

}
```

Vậy thôi, `@Repository` đánh dấu `UserRepository` là một Bean và chịu trách nhiệm giao tiếp với DB. **Spring Boot** sẽ tự tìm thấy và khởi tạo ra đối tượng `UserRepository` trong Context. Việc tạo ra `UserRepository` hoàn toàn tự động và tự config, vì chúng ta đã kế thừa `JpaRepository`. Bây giờ, việc lấy ra toàn bộ `User` sẽ như sau:

```java
@Autowired
UserRepository userRepository;

userRepository.findAll().forEach(System.out::println);
```

Nếu bạn tìm kiếm thì sẽ thấy `UserRepository` có hàng chục method mà chúng ta không cần viết lại nữa. Vì nó kế thừa `JpaRepository` rồi.
# Spring JPA Method + @Query

### **Query Creation**

Trong **Spring JPA**, có một cơ chế giúp chúng ta tạo ra các câu Query mà không cần viết thêm code. Cơ chế này xây dựng Query từ tên của method. Ví dụ: Chúng ta có đối tượng `User`.

_User.java_

```java
@Entity
@Table(name = "user")
@Data
public class User implements Serializable {
    private static final long serialVersionUID = -297553281792804396L;

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    // Mapping thông tin biến với tên cột trong Database
    @Column(name = "hp")
    private int hp;
    @Column(name = "stamina")
    private int stamina;

    // Nếu không đánh dấu @Column thì sẽ mapping tự động theo tên biến
    private int atk;
    private int def;
    private int agi;
}
```

Khi chúng ta đặt tên method là: `findByAtk(int atk)`. Thì **Spring JPA** sẽ tự định nghĩa câu Query cho method này, bằng cách xử lý tên method. Vậy là chúng ta đã có thể truy vấn dữ liệu mà chỉ mất thêm 1 dòng code.

### **Quy tắc đặt tên method trong Spring JPA**

Trong **Spring JPA**, cơ chế xây dựng truy vấn thông qua tên của method được quy định bởi các tiền tố sau:

`find…By`, `read…By`, `query…By`, `count…By`, và `get…By`.

phần còn lại sẽ được parse theo tên của thuộc tính (viết hoa chữ cái đầu). Nếu chúng ta có nhiều điều kiện, thì các thuộc tính có thể kết hợp với nhau bằng chữ `And` hoặc `Or`. Bạn có thể đọc thêm [trong Docs của Spring: Query Creation](https://docs.spring.io/spring-data/jpa/reference/jpa/query-methods.html#jpa.query-methods.query-creation)

Ví dụ:

```java
interface PersonRepository extends JpaRepository<User, Long> {
    // Dễ
    // version rút gọn
    Person findByLastname(String lastname);
    // verson đầy đủ
    Person findPersonByLastname(String lastname);
    Person findAllByLastname(String lastname);
    // Trung bình
    List<Person> findByEmailAddressAndLastname(EmailAddress emailAddress, String lastname);
    // findDistinct là tìm kiếm và loại bỏ đi các đối tượng trùng nhau
    List<Person> findDistinctPeopleByLastnameOrFirstname(String lastname, String firstname);
    List<Person> findPeopleDistinctByLastnameOrFirstname(String lastname, String firstname);
    // IgnoreCase là tìm kiếm không phân biệt hoa thường, ví dụ ở đây tìm kiếm lastname
    // lastname sẽ không phân biệt hoa thường
    List<Person> findByLastnameIgnoreCase(String lastname);
    // AllIgnoreCase là không phân biệt hoa thường cho tất cả thuộc tính
    List<Person> findByLastnameAndFirstnameAllIgnoreCase(String lastname, String firstname);
    // OrderBy là cách sắp xếp thứ tự trả về
    // Sắp xếp theo Firstname ASC
    List<Person> findByLastnameOrderByFirstnameAsc(String lastname);
    // Sắp xếp theo Firstname Desc
    List<Person> findByLastnameOrderByFirstnameDesc(String lastname);
}
```

Các thuộc tính trong tên method phải thuộc về Class đó, nếu không sẽ gây ra lỗi. Tuy nhiên, trong một số trường hợp bạn có thể query bằng thuộc tính con. Ví dụ: Đói tượng `Person` có thuộc tính là `Address` và trong `Address` lại có `ZipCode`

```java
// person.address.zipCode
List<Person> findByAddressZipCode(ZipCode zipCode);
```

### **@Query**

Với cách sử dụng `@Query`, bạn sẽ có thể sử dụng câu truy vấn [JPQL](https://en.wikipedia.org/wiki/Jakarta_Persistence_Query_Language) (Hibernate) hoặc SQL thuần (raw SQL). Ví dụ:

```java
public interface UserRepository extends JpaRepository<User, Long> {
    // Khi được gắn @Query, thì tên của method không còn tác dụng nữa
    // Đây là JPQL
    @Query("select u from User u where u.emailAddress = ?1")
    User myCustomQuery(String emailAddress);
    // Đây là Native SQL
    @Query(value = "select * from User u where u.email_address = ?1", nativeQuery = true)
    User myCustomQuery2(String emailAddress);
}
```

Cách truyền tham số là gọi theo thứ tự các tham số của method bên dưới `?1`, `?2`. Nếu bạn không thích sử dụng `?{number}` thì có thể đặt tên cho tham số.

```java
public interface UserRepository extends JpaRepository<User, Long> {
    // JPQL
    @Query("SELECT u FROM User u WHERE u.status = :status and u.name = :name")
    User findUserByNamedParams(@Param("status") Integer status, @Param("name") String name);
    // Native SQL
    @Query(value = "SELECT * FROM Users u WHERE u.status = :status and u.name = :name", nativeQuery = true)
    User findUserByNamedParamsNative(@Param("status") Integer status, @Param("name") String name);
}
```
# Chi tiết Spring Boot + Thymeleaf + MySQL + i18n

### Tạo Database

_script.sql_

```sql
CREATE SCHEMA IF NOT EXISTS `todo_db` DEFAULT CHARACTER SET utf8mb4 ;

CREATE TABLE IF NOT EXISTS `todo_db`.`todo` (
  `id` INT(11) NOT NULL AUTO_INCREMENT,
  `title` VARCHAR(255) NULL DEFAULT NULL,
  `detail` VARCHAR(255) NULL DEFAULT NULL,
  PRIMARY KEY (`id`))
ENGINE = InnoDB
DEFAULT CHARACTER SET = utf8mb4;
```

Thêm 1 record vào DB

```sql
INSERT INTO `todo_db`.`todo` (`title`, `detail`) VALUES ('Làm bài tập', 'Hoàn thiện bài viết Spring Boot #13');
```

### Cấu hình ứng dụng

Cấu hình là phần cực kì quan trọng rồi, chúng ta phải cung cấp cho Spring Boot các thông tin về Database và Thymeleaf. Ngoài ra, tùy chỉnh một số thông tin để giúp chúng ta lập trình đơn giản hơn.
(Có thể đọc qua và tham khảo một số thông số có thể được sử dụng). Đây là _application.properties_

```yaml
#Chạy ứng dụng trên port 8085
server.port=8085
# Bỏ tính năng cache của thymeleaf để lập trình cho nhanh
spring.thymeleaf.cache=false
# Các message tĩnh sẽ được lưu tại thư mục i18n
spring.messages.basename=i18n/messages
# Bỏ properties này đi khi deploy
# Nó có tác dụng cố định ngôn ngữ hiện tại chỉ là Tiếng Việt
spring.mvc.locale-resolver=fixed
# Mặc định ngôn ngữ là tiếng việt
spring.mvc.locale=vi_VN
# Đổi thành tiếng anh bằng cách bỏ comment ở dứoi
#spring.mvc.locale=en_US
spring.datasource.url=jdbc:mysql://localhost:3306/todo_db?useSSL=false
spring.datasource.username=root
spring.datasource.password=root
## Hibernate Properties
# The SQL dialect makes Hibernate generate better SQL for the chosen database
spring.jpa.properties.hibernate.dialect = org.hibernate.dialect.MySQL5InnoDBDialect
# Hibernate ddl auto (create, create-drop, validate, update)
spring.jpa.hibernate.ddl-auto = update
```

### Tạo Model

Tạo model `Todo` liên kết tới bảng `todo` trong Database.

```java
import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;

import lombok.Data;

@Entity
@Data
public class Todo {

}
```

Ngoài ra, chúng ta tạo thêm một đối tượng là `TodoValidator`, có trách nhiệm kiểm tra xem một object `Todo` là hợp lệ hay không.

```java
import org.thymeleaf.util.StringUtils;

/*
Đối tượng này dùng để kiểm tra xem một Object Todo có hợp lệ không
 */
public class TodoValidator {

    /
     * Kiểm tra một object Todo có hợp lệ không
     * @param todo
     * @return
     */
    public boolean isValid(Todo todo) {
        return Optional.ofNullable(todo)
                       .filter(t -> !StringUtils.isEmpty(t.getTitle())) // Kiểm tra title khác rỗng
                       .filter(t -> !StringUtils.isEmpty(t.getDetail())) // Kiểm tra detail khác rỗng
                       .isPresent(); // Trả về true nếu hợp lệ, ngược lại false
    }
}
```

Vậy là xong phần chuẩn bị `Model`.

### TodoConfig

Trong ứng dụng của mình, tôi muốn tự tạo ra Bean `TodoValidator`. Đây là lúc sử dụng `@Configuration` và `@Bean` đã học tại bài Spring Boot #6

_config/TodoConfig.java_

```java
@Configuration
public class TodoConfig {
    /
     * Tạo ra Bean TodoValidator để sử dụng sau này
     * @return
     */
    @Bean
    public TodoValidator validator() {
        return new TodoValidator();
    }
}
```

### Tầng Repository

Tầng Repository, chịu trách nhiệm giao tiếp với Database. Chúng ta sử dụng Spring JPA. _repository/TodoRepository.java_

```java
@Repository
public interface TodoRepository extends JpaRepository<Todo, Long> {
}
```

### Tầng Service

Tầng Service, chị trách nhiệm thực hiện các xử lý logic, business, hỗ trợ cho tầng Controller. _service/TodoService.java_

```java
@Service
public class TodoService {
    @Autowired
    private TodoRepository todoRepository;

    @Autowired
    private TodoValidator validator;

    /
     * Lấy ra danh sách List<Todo>
     *
     * @param limit - Giới hạn số lượng lấy ra
     *
     * @return Trả về danh sách List<Todo> dựa theo limit, nếu limit == null thì trả findAll()
     */
    public List<Todo> findAll(Integer limit) {
        return Optional.ofNullable(limit)
                       .map(value -> todoRepository.findAll(PageRequest.of(0, value)).getContent())
                       .orElseGet(() -> todoRepository.findAll());
    }

    /
     * Thêm một Todo mới vào danh sách công việc cần làm
     *
     * @return Trả về đối tượng Todo sau khi lưu vào DB, trả về null nếu không thành công
     */
    public Todo add(Todo todo) {
        if (validator.isValid(todo)) {
            return todoRepository.save(todo);
        }
        return null;
    }
}
```

### Tầng Controller

Tầng Controller, nơi đón nhận các request từ phía người dùng, và chuyển tiếp xử lý xuống tầng Service.

_controller/TodoController.java_

```java
@Controller
public class TodoController {

    @Autowired
    private TodoService todoService;

    /*
    @RequestParam dùng để đánh dấu một biến là request param trong request gửi lên server.
    Nó sẽ gán dữ liệu của param-name tương ứng vào biến
     */
    @GetMapping("/listTodo")
    public String index(Model model, @RequestParam(value = "limit", required = false) Integer limit) {
        // Trả về đối tượng todoList.
        model.addAttribute("todoList", todoService.findAll(limit));
        // Trả về template "listTodo.html"
        return "listTodo";
    }

    @GetMapping("/addTodo")
    public String addTodo(Model model) {
        model.addAttribute("todo", new Todo());
        return "addTodo";
    }

    /*
    @ModelAttribute đánh dấu đối tượng Todo được gửi lên bởi Form Request
     */
    @PostMapping("/addTodo")
    public String addTodo(@ModelAttribute Todo todo) {
        return Optional.ofNullable(todoService.add(todo))
                       .map(t -> "success") // Trả về success nếu save thành công
                       .orElse("failed"); // Trả về failed nếu không thành công

    }
}
```

### Templates

Tầng Controller đã trả về templates, nhiệm vụ tiếp theo là sử dụng Template Engine để xử lý các templates này và trả về webpage cho người dùng.

### i18n

Trong các template, tôi có sử dụng các message tĩnh, những message này hỗ trợ đa ngôn ngữ. Chúng ta định nghĩa các message này tại thư mục `i18n`.

*i18n/messages_vi.properties*

```
loda.message.hello=Welcome to TodoApp
loda.message.success=Thêm Todo thành công!
loda.message.failed=Thêm Todo không thành công!

loda.value.addTodo=Thêm công việc
loda.value.viewListTodo=Xem danh sách công việc
loda.value.listTodo=Danh sách công việc
```

*i18n/messages_en.properties*

```
loda.message.hello=Welcome to TodoApp
loda.message.success=Add To-do Successfully!
loda.message.failed=Add To-do Failed!

loda.value.addTodo=Add To-do
loda.value.viewListTodo=View To-do list
loda.value.listTodo=To-do list
```# Restful API + @RestController + @PathVariable + @RequestBody

### Restful API

Bạn có thể đọc [dwyl/learn-api-design](https://github.com/dwyl/learn-api-design)

### @RestController

Khác với `@Controller` là sẽ trả về một template.

`@RestController` trả về dữ liệu dưới dạng JSON.

```
@RestController
@RequestMapping("/api/v1")
public class RestApiController{

    @GetMapping("/todo")
    public List<Todo> getTodoList() {
        return todoList;
    }
}
```

Các đối tượng trả về dưới dạng Object sẽ được Spring Boot chuyển thành JSON. Các đối tượng trả về rất đa dạng, bạn có thể trả về `List`, `Map`, v.v.. Spring Boot sẽ convert hết chúng thành JSON, mặc định sẽ dùng Jackson converter để làm điều đó. Nếu bạn muốn API tùy biến được kiểu dữ liệu trả về, bạn có thể trả về đối tượng `ResponseEntity` của Spring cung cấp. Đây là đối tượng cha của mọi response và sẽ wrapper các object trả về. Cái này bạn xem tiếp phần dưới sẽ rõ.

### @RequestBody

Vì bây giờ chúng ta xây dựng API, nên các thông tin từ phía Client gửi lên Server sẽ nằm trong `Body`, và cũng dưới dạng `JSON` luôn.

```java
@RestController
@RequestMapping("/api/v1")
public class RestApiController {

    List<Todo> todoList = new CopyOnWriteArrayList<>();

    @PostMapping("/todo")
    public ResponseEntity addTodo(@RequestBody Todo todo) {
        todoList.add(todo);
        // Trả về response với STATUS CODE = 200 (OK)
        // Body sẽ chứa thông tin về đối tượng todo vừa được tạo.
        return ResponseEntity.ok().body(todo);
    }
}
```

Tất nhiên là Spring Boot sẽ làm giúp chúng ta các phần nặng nhọc, nó chuyển chuỗi JSON trong request thành một Object Java. bạn chỉ cần cho nó biết cần chuyển JSON thành Object nào bằng Annotation `@RequestBody`

### @PathVariable

`RESTful API` là một tiêu chuẩn dùng trong việc thết kế các thiết kế API cho các ứng dụng web để quản lý các resource.

Và với cách thống nhất này, thì sẽ có một phần thông tin quan trọng sẽ nằm ngay trong chính URL của api.

Ví dụ:

1. URL tạo To-do: https://loda.me/todo. Tương ứng với HTTP method là POST
2. URL lấy thông tin To-do số 12: https://loda.me/todo/12. Tương ứng với HTTP method là GET
3. URL sửa thông tin To-do số 12: https://loda.me/todo/12. Tương ứng với HTTP method là PUT
4. URL xoá To-do số 12: https://loda.me/todo/12. Tương ứng với HTTP method là DELETE

Ngoài thông tin trong `Body` của request, thì cái chúng ta cần chính là cái con số 12 nằm trong URL. Phải lấy được con số đó thì mới biết được đối tượng To-do cần thao tác là gì.

`@PathVariable` tham chiến.

```java
@RestController
@RequestMapping("/api/v1")
public class RestApiController {

    /*
    phần path URL bạn muốn lấy thông tin sẽ để trong ngoặc kép {}
     */
    @GetMapping("/todo/{todoId}")
    public Todo getTodo(@PathVariable(name = "todoId") Integer todoId){
        // @PathVariable lấy ra thông tin trong URL
        // dựa vào tên của thuộc tính đã định nghĩa trong ngoặc kép /todo/{todoId}
        return todoList.get(todoId);
    }
}
```# Exception Handling @ExceptionHandler + @RestControllerAdvice/@ControllerAdvice + @ResponseStatus

### @RestControllerAdvice & @ControllerAdvice + @ExceptionHandler

`@RestControllerAdvice` là một Annotation gắn trên Class. Có tác dụng xen vào quá trình xử lý của các `@RestController`. Tương tự với `@ControllerAdvice`. `@RestControllerAdvice` thường được kết hợp với `@ExceptionHandler` để cắt ngang quá trình xử lý của Controller, và xử lý các ngoại lệ xảy ra.

```java
@RestControllerAdvice
public class ApiExceptionHandler {
    @ExceptionHandler(IndexOutOfBoundsException.class)
    @ResponseStatus(value = HttpStatus.BAD_REQUEST)
    public ErrorMessage TodoException(Exception ex,  WebRequest request) {
        return new ErrorMessage(10100, "Đối tượng không tồn tại");
    }
}
```

Hiểu đơn giản là Controller đang hoạt động bình thường, chẳng may có một Exception được ném ra, thì thay vì báo lỗi hệ thống, thì nó sẽ được thằng `@RestControllerAdvice` và `@ExceptionHandler` đón lấy và xử lý. Sau đó trả về cho người dùng thông tin hữu ích hơn.

### @ResponseStatus

`@ResponseStatus` là một cách định nghĩa HttpStatus trả về cho người dùng. Nếu bạn không muốn sử dụng `ResponseEntity` thì có thể dùng `@ResponseStatus` đánh dấu trên `Object` trả về.
# Hướng dẫn sử dụng @ConfigurationProperties

### **Cấu hình đơn giản**

Giả sử, ứng dụng của tôi sẽ yêu cầu có một số giá trị toàn cục, mà thay vì cấu hình ở trong code, tôi muốn lưu nó ở bên ngoài, để tiện thay đổi mỗi khi cần.

Thì tôi sẽ làm như sau, tạo ra một class chứa các thông tin:

```java
@Data // Lombok (Tạo Getter, Setter các thứ)
@Component // Là 1 Spring Bean
// Đánh dấu để lấy config từ trong file loda.yml
// @PropertySource("classpath:loda.yml")
@ConfigurationProperties(prefix = "loda") // Chỉ lấy các config có tiền tố là "loda"
public class LodaAppProperties {
    private String email;
    private String googleAnalyticsId;

    // standard getters and setters
}
```

Sử dụng `@PropertySource` để định nghĩa tên của file config. Nếu không có annotation này, Spring sẽ sử dụng file mặc định (_classpath:application.yml_ trong thư mục _resources_)

Cuối cùng là `@ConfigurationProperties`, Annotation này đánh dấu class bên dưới nó là properties, các thuộc tính sẽ được tự động nạp vào khi Spring khởi tạo. Lưu ý: các thuộc tính này được xác định bởi `prefix=loda`. Cái này bạn xem file _application.yml_ ở dưới sẽ hiểu.

Spring sẽ tự tìm các hàm setter để set giá trị cho các thuộc tính này, nên quan trọng là bạn phải tạo ra các setter method. (Ở đây tôi nhường việc đó cho lombok).

Ngoài ra, để chạy được tính năng này, bạn cần kích hoạt nó bằng cách gắn `@EnableConfigurationProperties` lên một Configuration nào đó. Ở đây tôi gắn lên hàm main luôn.

```java
@SpringBootApplication
@EnableConfigurationProperties
public class App {
    public static void main(String[] args) {
        SpringApplication.run(App.class, args);
    }
}
```

Bây giờ, Spring sẽ tự động bind toàn bộ giá trị từ trong file application.yml vào bean LodaAppProperties cho chúng ta. Tạo ra file _application.yml_ tại thư mục resources. Thêm các thông tin chúng ta cần. Chúng ta phải đặt các thuộc tính này sau prefix _loda_:

```
loda:
  email: loda.namnh@gmail.com
  googleAnalyticsId: U-xxxxx
```


### **Chạy thử**

```java
@SpringBootApplication
@EnableConfigurationProperties
public class App implements CommandLineRunner {
    public static void main(String[] args) {
        SpringApplication.run(App.class, args);
    }

    @Autowired LodaAppProperties lodaAppProperties;

    @Override
    public void run(String... args) throws Exception {
        System.out.println("Global variable:");
        System.out.println("\t Email: "+lodaAppProperties.getEmail());
        System.out.println("\t GA ID: "+lodaAppProperties.getGoogleAnalyticsId());
    }
}
```

Kết quả:

```
Global variable:
	 Email: loda.namnh@gmail.com
	 GA ID: U-xxxxx
```

Bây giờ, ở bất kỳ đâu trong chương trình, khi cần lấy các thông tin config, tôi chỉ cần:

```java
@Autowired LodaAppProperties lodaAppProperties;
```

### **Nested Properties**

Chúng ta có thể config các thuộc tính bên trong Class kể cả khi nó là `Lists`, `Maps` hay một class khác.

Bổ sung thêm thuộc tính:

```java
...
@ConfigurationProperties(prefix = "loda") // Chỉ lấy các config có tiền tố là "loda"
public class LodaAppProperties {
    ...
    private List<String> authors;
    private Map<String, String> exampleMap;
}
```

Sửa file _application.yml_:

```
loda:
  email: loda.namnh@gmail.com
  googleAnalyticsId: U-xxxxx
  authors:
    - loda
    - atom
  exampleMap:
    key1: hello
    key2: world
```

Chạy lại chương trình:

```java
@Override
public void run(String... args) throws Exception {
    System.out.println("Global variable:");
    System.out.println("\t Email: " + lodaAppProperties.getEmail());
    System.out.println("\t GA ID: " + lodaAppProperties.getGoogleAnalyticsId());
    System.out.println("\t Authors: " + lodaAppProperties.getAuthors());
    System.out.println("\t Example Map: " + lodaAppProperties.getExampleMap());
}
```

Kết quả:

```
Global variable:
	 Email: loda.namnh@gmail.com
	 GA ID: U-xxxxx
	 Authors: [loda, atom]
	 Example Map: {key1=hello, key2=world}
```# Chạy nhiều môi trường với Spring Profile

### 1. Tạo file config

`Spring Profiles` có sẵn trong Framework rồi nên bạn không cần thêm bất kì thư viện nào khác. Để sử dụng, các bạn tạo file config tại thư mục `resources` trong project. Mặc định Spring sẽ nhận các file có tên như sau:

```yaml
application.properties
application.yml
application-{profile-name}.yml // .properties
```

ví dụ mình có 2 môi trường là `local` và `aws`, thì mình sẽ tạo ra các file như thế này:

```yaml
application.yml
application-local.yml
application-aws.yml
application-common.yml
```

- `application` là file config chính khai báo các enviroment.
- `application-local` chỉ sử dụng khi chạy chương trình ở local
- `application-aws` chỉ sử dụng khi chạy ở AWS
- `application-common` là những config dùng chung, môi trường nào cũng cần.

Bây giờ, mình sẽ khai báo trong từng file như sau (Xóa bớt nội dung từng tệp rùi cho ngắn gọn):

_application.yml_

```yml
#application.yml
---
spring.profiles: local
spring.profiles.include: common, local
---
spring.profiles: aws
spring.profiles.include: common, aws
---
```

_application-aws.yml_

```yml
spring:
  datasource:
    username: xxx
    password: xxx
```

_application-local.yml_

```yml
spring:
  datasource:
    username: root
    password:
    url: jdbc:mysql://localhost:3306/news?useSSL=false&characterEncoding=UTF-8
```

_application-common.yml_

```yml
spring:
  jpa:
    properties:
```

Tadaa, xong, mình giải thích chút. bạn để ý trong file `application.yml` mình có khai báo 2 môi trường là `local` và `aws`. Tại mỗi môi trường sẽ `include` (bao gồm) các file config như kia. Khi mình kích hoạt `aws` chẳng hạn, _Spring_ sẽ load tất cả config có trong `application-common.yml` và `application-aws.yml`.

### 2. Kích hoạt config

Để sử dụng một `Profiles` bạn có các cách sau:

#### Sử dụng `spring.profiles.active` trong file `application.properties` hoặc `application.yml`

```
spring.profiles.active=aws
```

#### 3: Sử dụng JVM System Parameter (nên dùng)

```
-Dspring.profiles.active=aws
```

#### 4: Environment Variable (Unix) (nên dùng)

```
export SPRING_PROFILES_ACTIVE=aws
```

### 3. Cách sử dụng @Profile

Khi đã có `Profile` rồi, ngoài các biến toàn cục được thay đổi theo môi trường, bạn cũng có thể toàn quyền quyết định xem trong code rằng `Bean` hay `Class` nào sẽ được quyền chạy ở môi trường nào. Bằng cách sử dụng annotation `@Profile`

```java
// Bean này Spring chỉ khởi tạo và quản lý khi môi trường là `local`
@Component
@Profile("local")
public class LocalDatasourceConfig {}
// Ngoài ra bạn có thể sử dụng toàn tử logic ở đây, ví dụ:
@Component
@Profile("!local")
public class LocalDatasourceConfig {}
```# Hướng dẫn chi tiết Test Spring Boot

Hôm nay chúng ta sẽ tìm hiểu cách để viết TestCase trong **Spring Boot**. Yêu cầu tối thiếu để đi tiếp phần này đó là bạn phải có nền tảng với `JUnit` và `Mockito`.

### **Vấn đề Test + Spring**

**Spring Boot** sẽ phải tạo Context và tìm kiếm các Bean và nhét vào đó. Sau tất cả các bước config và khởi tạo thì chúng ta sử dụng `@Autowired` để lấy đối tượng ra sử dụng. Vấn đề đầu tiên bạn nghĩ tới khi viết Test sẽ là làm sao `@Autowired` bean vào class Test được và làm sao cho `JUnit` hiểu `@Autowired` là gì. Hướng giải quyết là tích hợp `Spring` vào với `JUnit`.

### **@RunWith(SpringRunner.class)**

**Spring Boot** đã thiết kế ra lớp `SpringRunner`, sẽ giúp chúng ta tích hợp **Spring + JUnit**.

Để test trong Spring, trong mọi class Test chúng ta sẽ thêm `@RunWith(SpringRunner.class)` lên đầu Class Test đó.

```
@RunWith(SpringRunner.class)
public class TodoServiceTest {
    ...
}
```

Khi bạn chạy `TodoServiceTest` nó sẽ tạo ra một `Context` riêng để chứa `bean` trong đó, vậy là chúng ta có thể `@Autowired` thoải mái trong nội hàm Class này.

Vấn đề tiếp theo là làm sao đưa `Bean` vào trong `Context`.

Có 2 cách

1. `@SpringBootTest`
2. `@TestConfiguration`

### **@SpringBootTest**

`@SpringBootTest` sẽ đi tìm kiếm class có gắn `@SpringBootApplication` và từ đó đi tìm toàn bộ `Bean` và nạp vào `Context`.

Cái này chỉ nên sử dụng trong trường hợp muốn Integration Tests, vì nó sẽ tạo toàn bộ `Bean`, không khác gì bạn chạy cả cái `SpringApplication.run(App.class, args);`, rất tốn thời gian mà rất nhiều `Bean` thừa thãi, không cần dùng đến cũng khởi tạo.

```
@RunWith(SpringRunner.class)
@SpringBootTest
public class TodoServiceTest {

    /**
     * Cách này sử dụng @SpringBootTest
     * Nó sẽ load toàn bộ Bean trong app của bạn lên,
     * Giống với việc chạy App.java vậy
     */

    @Autowired
    private TodoService todoService;
}
```

### **@TestConfiguration**

`@TestConfiguration` giống với `@Configuration`, chúng ta tự định nghĩa ra `Bean`.

Các Bean được tạo bởi `@TestConfiguration` chỉ tồn tại trong môi trường Test. Rất phụ hợp với việc viết UnitTest.

Class Test nào, cần Bean gì, thì tự tạo ra trong `@TestConfiguration`

```
@RunWith(SpringRunner.class)
public class TodoServiceTest2 {

    /**
     * Cách này sử dụng @TestConfiguration
     * Nó chỉ tạo ra Bean trong Context test này mà thôi
     * Tiết kiệm thời gian hơn khi sử dụng @SpringBootTest (vì phải load hết Bean lên mà không dùng đến)
     */
    @TestConfiguration
    public static class TodoServiceTest2Configuration{

        /*
        Tạo ra trong Context một Bean TodoService
         */
        @Bean
        TodoService todoService(){
            return new TodoService();
        }
    }

    @Autowired
    private TodoService todoService;
```

### **@MockBean**

**Spring** hỗ trợ mock với annotation `@MockBean`, chúng ta có thể mock lấy ra một `Bean` "giả" mà không thèm để ý tới thằng `Bean` "thật". (Kể cả thằng "thật" có ở trong Context đi nữa, cũng không quan tâm).

```
@RunWith(SpringRunner.class)
public class TodoServiceTest2 {

    /**
     * Đối tượng TodoRepository sẽ được mock, chứ không phải bean trong context
     */
    @MockBean
    TodoRepository todoRepository;
```

### **Demo 1**

Chúng ta sẽ học cách sử dụng các Annotation ở trên.

### **Cài đặt**

_pom.xml_

```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <packaging>pom</packaging>
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.0.5.RELEASE</version>
        <relativePath /> <!-- lookup parent from repository -->
    </parent>
    <groupId>me.loda.spring</groupId>
    <artifactId>spring-boot-learning</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <name>spring-boot-learning</name>
    <description>Everything about Spring Boot</description>

    <properties>
        <java.version>1.8</java.version>
    </properties>

    <dependencies>

        <!--spring mvc, rest-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <!--for test-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
    </dependencies>

    <build>
        <plugins>
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
        </plugin>
        </plugins>
    </build>

</project>
```

Cấu trúc thư mục:

!image

### **Tạo Model, Service, Repository**

Chúng ta sử dụng Lombok cho tiện nhé.

_Todo.java_

```
@Data
@AllArgsConstructor
public class Todo {
    private int id;
    private String title;
    private String detail;
}
```

Vì phục vụ mục đích demo, chúng ta sẽ không cần sử dụng tới **Spring JPA** mà sẽ tự viết.

_TodoRepository.java_

```
public interface TodoRepository {
    List<Todo> findAll();

    Todo findById(int id);
}
```

_TodoService.java_

```
@Service
public class TodoService {
    @Autowired
    private TodoRepository todoRepository;

    public int countTodo(){
        return todoRepository.findAll().size();
    }

    public Todo getTodo(int id){
        return todoRepository.findById(id);
    }

    public List<Todo> getAll(){
        return todoRepository.findAll();
    }
}
```

_App.java_

```
@SpringBootApplication
public class App {
    public static void main(String[] args) {
        SpringApplication.run(App.class, args);
    }
}
```

Rất đơn giản.

### **Test bằng @SpringBootTest**

Chúng ta `mock TodoRepository` và giả lập cho nó trả ra một `List<Todo>` gồm 10 phần tử.

```
@RunWith(SpringRunner.class)
@SpringBootTest
public class TodoServiceTest {

    /**
     * Cách này sử dụng @SpringBootTest
     * Nó sẽ load toàn bộ Bean trong app của bạn lên,
     * Giống với việc chạy App.java vậy
     */

    /**
     * Đối tượng TodoRepository sẽ được mock, chứ không phải bean trong context
     */
    @MockBean
    TodoRepository todoRepository;

    @Autowired
    private TodoService todoService;

    @Before
    public void setUp() {
        Mockito.when(todoRepository.findAll())
               .thenReturn(IntStream.range(0, 10)
                                    .mapToObj(i -> new Todo(i, "title-" + i, "detail-" + i))
                                    .collect(Collectors.toList()));

    }

    @Test
    public void testCount() {
        Assert.assertEquals(10, todoService.countTodo());
    }

}
```

Bạn sẽ thấy Test chạy thành công, nhưng sẽ mất thời gian vì khởi tạo `Context` quá lâu do `@SpringBootTest` phải load hết tất cả bean lên.

### **Test bằng @TestConfiguration**

```
```java
package me.loda.spring.testinginspringboot;
/*******************************************************
 * For Vietnamese readers:
 *    Các bạn thân mến, mình rất vui nếu project này giúp
 * ích được cho các bạn trong việc học tập và công việc. Nếu
 * bạn sử dụng lại toàn bộ hoặc một phần source code xin để
 * lại dường dẫn tới github hoặc tên tác giá.
 *    Xin cảm ơn!
 *******************************************************/

import java.util.stream.Collectors;
import java.util.stream.IntStream;

import org.junit.Assert;
import org.junit.Before;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.mockito.Mockito;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.TestConfiguration;
import org.springframework.boot.test.mock.mockito.MockBean;
import org.springframework.context.annotation.Bean;
import org.springframework.test.context.junit4.SpringRunner;

@RunWith(SpringRunner.class)
public class TodoServiceTest2 {

    /**
     * Cách này sử dụng @TestConfiguration
     * Nó chỉ tạo ra Bean trong Context test này mà thôi
     * Tiết kiệm thời gian hơn khi sử dụng @SpringBootTest (vì phải load hết Bean lên mà không dùng đến)
     */
    @TestConfiguration
    public static class TodoServiceTest2Configuration{

        /*
        Tạo ra trong Context một Bean TodoService
         */
        @Bean
        TodoService todoService(){
            return new TodoService();
        }
    }

    @MockBean
    TodoRepository todoRepository;

    @Autowired
    private TodoService todoService;

    @Before
    public void setUp() {
        Mockito.when(todoRepository.findAll())
               .thenReturn(IntStream.range(0, 10)
                                    .mapToObj(i -> new Todo(i, "title-" + i, "detail-" + i))
                                    .collect(Collectors.toList()));

    }

    @Test
    public void testCount() {
        Assert.assertEquals(10, todoService.countTodo());
    }

}
```

### **Vấn đề Test + Spring Boot 2**

Ở trên chúng ta đã xử lý xong vấn đề dependency injection của **JUnit** + **Spring Boot** rồi.

Vấn đề tiếp theo là cái **Controller**. Đúng vậy, chúng ta tạo ra hàng chục Rest API đón request người dùng, vậy làm sao để test nó.

Nếu **Controller** không được test thì là một lỗ hổng cực lớn, vì nó là đầu ra chính của chương trình, nó sai -> chương trình không có giá trị.

Tuy nhiên, chúng ta không muốn khởi động cả Tomcat Server + Database để test 1 API. Nó rất tốn tài nguyên và thời gian.

### **@WebMvcTest**

**Spring Boot** hỗ trợ test **Controller** mà không cần khởi động Tomcat Server bằng annotation `@WebMvcTest`.

Tất nhiên là nếu không khởi động Server, thì phải có một phương thức khác giả lập, vâng, lại là **Mock**.

Bây giờ, khi muốn test một Controller nào đó, chúng ta làm như sau:

```
@RunWith(SpringRunner.class)
// Bạn cần cung cấp lớp Controller cho @WebMvcTest
@WebMvcTest(TodoRestController.class)
public class TodoRestControllerTest {
    /**
     * Đối tượng MockMvc do Spring cung cấp
     * Có tác dụng giả lập request, thay thế việc khởi động Server
     */
    @Autowired
    private MockMvc mvc;
}
```

### **Demo 2**

Chúng ta vẫn sử dụng tiếp Project đã tạo ở **\# Demo 1**

### **Tạo Controller**

_TodoRestController.java_

```
@RestController
@RequestMapping("/api/v1")
public class TodoRestController {
    @Autowired
    TodoService todoService;

    @GetMapping("/todo")
    public List<Todo> findAll(){
        return todoService.getAll();
    }
}
```

### **Tạo Test Controller**

```
import static org.hamcrest.CoreMatchers.is;
import static org.hamcrest.Matchers.hasSize;
import static org.mockito.BDDMockito.given;
import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.get;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.jsonPath;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.status;

import java.util.List;
import java.util.stream.Collectors;
import java.util.stream.IntStream;

import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.autoconfigure.web.servlet.WebMvcTest;
import org.springframework.boot.test.mock.mockito.MockBean;
import org.springframework.http.MediaType;
import org.springframework.test.context.junit4.SpringRunner;
import org.springframework.test.web.servlet.MockMvc;

@RunWith(SpringRunner.class)
// Bạn cần cung cấp lớp Controller cho @WebMvcTest
@WebMvcTest(TodoRestController.class)
public class TodoRestControllerTest {
    /**
     * Đối tượng MockMvc do Spring cung cấp
     * Có tác dụng giả lập request, thay thế việc khởi động Server
     */
    @Autowired
    private MockMvc mvc;

    @MockBean
    private TodoService todoService;

    @Test
    public void testFindAll() throws Exception {
        // Tạo ra một List<Todo> 10 phần tử
        List<Todo> allTodos = IntStream.range(0, 10)
                                       .mapToObj(i -> new Todo(i, "title-" + i, "detail-" + i))
                                       .collect(Collectors.toList());

        // giả lập todoService trả về List mong muốn
        given(todoService.getAll()).willReturn(allTodos);

        mvc.perform(get("/api/v1/todo").contentType(MediaType.APPLICATION_JSON)) // Thực hiện GET REQUEST
           .andExpect(status().isOk()) // Mong muốn Server trả về status 200
           .andExpect(jsonPath("$", hasSize(10))) // Hi vọng server trả về List độ dài 10
           .andExpect(jsonPath("$[0].id", is(0))) // Hi vọng phần tử trả về đầu tiên có id = 0
           .andExpect(jsonPath("$[0].title", is("title-0"))) // Hi vọng phần tử trả về đầu tiên có title = "title-0"
           .andExpect(jsonPath("$[0].detail", is("detail-0")));
    }
}
```

Chạy thử và trải nghiệm bạn nhé :3

### **Kết**

Test là một phần quan trọng trong hệ thống, hi vọng các đọc kĩ và thực hành để nắm chắc.

💁 Nếu có, toàn bộ project / code mẫu được lưu trữ tại **GitHub**

🌟 Đây là một bài viết trong Series **Làm chủ Spring Boot – Zero to Hero**

# Hướng dẫn chi tiết Test Spring Boot (Phần 2)

Trong bài này, tôi sẽ tập trung vào việc giới thiệu với các các bạn cách chuẩn bị dữ liệu test.

Bài viết có sử dụng:

1. Hibernate là gì?
2. Spring JPA
3. Lombok
4. Spring Boot

### **Cài đặt**

```
<?xml version="1.0" encoding="UTF-8"?>
<projectxmlns="http://maven.apache.org/POM/4.0.0"xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"xsi:schemaLocation="http://maven.apache.org/POM/4.0.0https://maven.apache.org/xsd/maven-4.0.0.xsd"><modelVersion>4.0.0</modelVersion><parent><groupId>org.springframework.boot</groupId><artifactId>spring-boot-starter-parent</artifactId><version>2.0.5.RELEASE</version><relativePath/> <!-- lookup parent from repository -->
	</parent><groupId>me.loda.spring</groupId><artifactId>example-independent-maven-spring-project</artifactId><version>0.0.1-SNAPSHOT</version><name>example-independent-maven-spring-project</name><description>Demo project for Spring Boot</description><properties><java.version>1.8</java.version></properties><dependencies><dependency><groupId>org.springframework.boot</groupId><artifactId>spring-boot-starter-web</artifactId></dependency><dependency><groupId>org.springframework.boot</groupId><artifactId>spring-boot-devtools</artifactId><scope>runtime</scope><optional>true</optional></dependency><dependency><groupId>org.projectlombok</groupId><artifactId>lombok</artifactId><optional>true</optional></dependency><dependency><groupId>org.springframework.boot</groupId><artifactId>spring-boot-starter-test</artifactId><scope>test</scope></dependency><!--spring jpa-->
		<dependency><groupId>org.springframework.boot</groupId><artifactId>spring-boot-starter-data-jpa</artifactId></dependency><!--in memory database-->
		<dependency><groupId>com.h2database</groupId><artifactId>h2</artifactId><scope>runtime</scope></dependency></dependencies><build><plugins><plugin><groupId>org.springframework.boot</groupId><artifactId>spring-boot-maven-plugin</artifactId></plugin></plugins></build></project>
```

### **Prepare**

Trước hết, giả sử chúng ta có một hệ thống quản lý công việc

_Todo.java_

```
@Data
@AllArgsConstructor
@NoArgsConstructor
@Entity
public class Todo {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private int id;
    private String title;
    private String detail;
}
```

_TodoRepository.java_

```
public interface TodoRepository extends JpaRepository<Todo, Long> {
    List<Todo> findAll();

    Todo findById(int id);
}
```

_TodoController.java_

```
@RestController
@RequestMapping("/api/v1")
@RequiredArgsConstructor
public class TodoRestController {
    private final TodoService todoService;

    @GetMapping("/todo")
    public List<Todo> findAll() {
        return todoService.getAll();
    }
}
```

### **@DataJpaTest**

Về cơ bản, chúng ta không thể mock hay làm giả dữ liệu của Database mãi được, nó sẽ là một lỗ hổng lớn trong hệ thống. Ngoài ra, bạn cũng sẽ không thể test được quá trình thao tác với Database của dự án.

Vậy nên sau cùng, chúng ta cũng sẽ phải test các thao tác vận hành với Database,

Nhưng chớ lo, để tập trung vào việc test các thao tác với Database, Spring Boot đã hỗ trợ chúng ta bằng `@DataJpaTest`

Kết hợp giữa `@DataJpaTest` và `h2-database` (Memory database) là Combo hoàn hảo.

```
@RunWith(SpringRunner.class)
/**
 * Khi đánh dấu một class là @DataJpaTest.
 * Spring Boot sẽ load lên tất cả các Bean có liên quan tới tầng Repository
 * Thay vì load hết tất cả Bean như là @SpringBootTest
 */
@DataJpaTest
public class DataJpaAnnotationTest {

    @Autowired
    private DataSource dataSource;
    @Autowired
    private JdbcTemplate jdbcTemplate;
    @Autowired
    private EntityManager entityManager;
    @Autowired
    private TodoRepository todoRepository;

    @Test
    public void allComponentAreNotNull() {
        Assertions.assertThat(dataSource).isNotNull();
        Assertions.assertThat(jdbcTemplate).isNotNull();
        Assertions.assertThat(entityManager).isNotNull();
        Assertions.assertThat(todoRepository).isNotNull();
    }

}
```

`@DataJpaTest` có tác dụng khởi tạo các Bean cần thiết cho tầng Repository. Ngoài ra, nó không khởi tạo thêm các Bean thừa thãi nào khác. (Chuyên biệt hơn `@SpringBootTest`)

### **Tạo Data**

Cách Fake dữ liệu đơn giản nhất là tự insert bằng repository

```
import org.assertj.core.api.Assertions;
import org.junit.After;

    @Test
    public void testTodoRepositoryByCode() {
        todoRepository.save(new Todo(0, "Todo-1", "Detail-1"));
        todoRepository.save(new Todo(0, "Todo-2", "Detail-2"));

        Assertions.assertThat(todoRepository.findAll()).hasSize(2);
        Assertions.assertThat(todoRepository.findById(1).getTitle()).isEqualTo("Todo-1");
    }

    @After
    public void clean() {
        todoRepository.deleteAll();
    }
```

### **@Sql**

Một cách làm hay hơn để chuẩn bị dữ liệu cho Test đó là sử dụng annotation `@Sql`

`@Sql` có tác dụng load một hoặc nhiều file scripts sql lên và thực thi.

ví dụ:

_createToDo.sql_

```
INSERT INTO todo (title, detail) VALUES ('Todo-1', 'Do homework');
INSERT INTO todo (title, detail) VALUES ('Todo-2', 'Walking');
```

Để chạy scripts này trong class Test, bạn làm như sau:

```
@RunWith(SpringRunner.class)
@DataJpaTest
public class SqlAnnotationTest {
    @Autowired
    private TodoRepository todoRepository;

    @Test
    @Sql("/createTodo.sql")
    public void testTodoRepositoryBySqlSchema() {
        Assertions.assertThat(todoRepository.findAll()).hasSize(2);
        Assertions.assertThat(todoRepository.findById(1).getTitle()).isEqualTo("Todo-1");
    }

}
```

Để chạy nhiều file sql cùng lúc, bạn sử dụng `@SqlGroup`

### **Kết**

💁 Nếu có, toàn bộ project / code mẫu được lưu trữ tại **GitHub**

🌟 Đây là một bài viết trong Series **Làm chủ Spring Boot – Zero to Hero**

# [SB20] Hướng dẫn toàn tập Mockito

### **Yêu cầu**

Trước khi tìm hiểu Mockito, bạn cần biết JUnit.

Trong bài viết có sử dụng Lombok

### **Giới thiệu**

Về cơ bản, Unit test là phương pháp tiếp cận độc lập, tức là mỗi một chức chăng năng sẽ được đi kèm với một hoặc nhiều bài test để chắc chắn ràng cái chức năng đó hoạt động ổn. Vậy nên Unit Test thường là đơn nhỏ nhất và test case cũng dễ dàng khởi tạo

```
@Test
public void testSum(){
    Assert.assertEquals(3, MathUtil.sum(1,2));
}
```

Bài toán mở rộng ra hơn, Khi chúng ta phải test sự hoạt động phối hợp giữa nhiều chức năng hay thành phần với nhau hoặc muốn test một chức năng lớn thì sẽ trở nên phức tạp và khó xây dựng hơn rất nhiều.

Các kịch bản sau thường diễn ra:

- Chức năng A gọi tới chức năng B. tuy nhiên, B chưa viết xong
- Chức năng A gọi tới chức năng B. tuy nhiên, B khởi tạo rất phức tạp (truy xuất Database, yêu cầu nhiều params, v.v.)
- Muốn test chức năng A, tuy nhiên A yêu cầu nỗ lực lớn của cả hệ thống (Yêu cầu có HTTP-server, authorize, v.v.)

```
// Giả sử muốn test hàm này
public int getAllData(){

    // Giả sử như thằng driver không được khởi tạo hoặc bị lỗi
    // thì function này coi như hỏng

    MySQLdriver.connect() // Kết nối xuống Database
    var data = MySQLdriver.get()// Lấy dữ liệu

    // Chúng ta muốn test logic xử lý dữ liệu ở dưới đây,
    // mà k cần kết nối databse, phải làm sao?

    // Xử lý dữ liệu
    ...
    // trả ra dữ liệu
    return data
}
```

Rất nhiều kịch bản phức tạp xảy ra, mà trên thực tế, chúng ta chỉ mong muốn confirm rằng A ổn, chứ thằng B, C, D gì đó thì hãy cứ tạm coi là nó "đã ổn" đi.

Để đơn giản hoá các kịch bản test như trên, khái niệm `Mock` ra đời.

Tư tưởng của `Mock` đơn giản là khi muốn test (A gọi B) thì thay vì tạo ra một đối tượng B thật sự, bạn tạo ra một thằng B' giả mạo, có đầy đủ chức năng như B thật (nhưng không phải thật)

Bạn sẽ giả lập cho B' biết là khi thằng A gọi tới nó, nó cần làm gì, trả lại cái gì (hardcode). Miễn làm sao cho nó trả ra đúng cái thằng A cần để chúng ta có thể test A thuận lợi nhất.

```
// Đại loại như này
// Khi driver.get() được gọi, hãy trả ra một List(1,2,3)
Mockito.when(driver.get())
       .thenReturn(Arrays.asList(1, 2, 3));
```

Ở trong Java, `Mockito` chính là thư viện nổi tiếng nhất để các bạn làm việc này.

### **Cài đặt**

Để sử dụng `Mockito` bạn cần:

```
<!--https://mvnrepository.com/artifact/org.mockito/mockito-core -->
<dependency><groupId>org.mockito</groupId><artifactId>mockito-core</artifactId><version>3.2.4</version><scope>test</scope></dependency>
```

Chúng ta đang viết test, nên đừng quên `JUnit` nữa nhé:

```
<!--https://mvnrepository.com/artifact/junit/junit -->
<dependency><groupId>junit</groupId><artifactId>junit</artifactId><version>4.12</version><scope>test</scope></dependency>
```

### **Cách Mock**

Khái niệm cơ bản đầu tiên, đó là làm sao để tạo ra một đối tượng bị Mock.

### **Cách 1: Mockito.mock()**

Sử dụng `Mockito.mock()` để tạo ra một object của `Class` bạn yêu cầu, nó thậm chí còn không quan tâm hàm khởi tạo của `Class` ý như nào và ra sao, vì nó đâu có thật :v

```
@Test
public void testUserMockFunction() {
    List mockList = Mockito.mock(List.class);

    Mockito.when(mockList.size()).thenReturn(2);

    Assert.assertEquals(2, mockList.size());
}
```

### **Cách 2: Sử dụng @Mock**

Bạn muốn mock cái gì, thì đánh dấu `@Mock` lên ý, đơn giản vãi :v

```
@Mock
List<String> mockedList;
```

Tuy nhiên, gắn `@Mock` là chưa đủ, bạn cần kích hoạt `Mockito` để nó mock các object ý cho bạn.

Sau khi kích hoạt, thì tất cả các object được gắn `@Mock` sẽ trở thành 1 object giả mạo, và đã được khởi tạo (tức là `!= null`)

Các cách kích hoạt như sau:

1. Gắn `@RunWith(MockitoJUnitRunner.class)` lên class test của bạn

```
// Cách 1
@RunWith(MockitoJUnitRunner.class)
public class MockitoAnnotationTest {
    @Mock
    List<String> mockedList;
}
```

1. tạo ra một đối tượng `MockitoRule` bên trong class test của bạn

```
public class MockitoAnnotationTest {

    // Cách 2
    @Rule
    public MockitoRule initRule = MockitoJUnit.rule();

    @Mock
    List<String> mockedList;
}
```

1. Sử dụng `Mockito.init()`

```
public class MockitoAnnotationTest {

    @Mock
    List<String> mockedList;

    @Before
    public void setUp() throws Exception {
        // Cách 3
        // Nếu bạn không dùng cách 1 hoặc 2 thì phải sử dụng
        // dòng code dưới đây:
        MockitoAnnotations.initMocks(this);
    }
}
```

Vậy là xong, bạn đã tạo ra được một Object bị mock.

### **Cách sử dụng**

`Mockito` cung cấp rất nhiều methods khác nhau để giúp bạn giả lập đầy đủ các thứ bạn cần.

Hay sử dụng nhất là hàm `when()`.

```
// Trả là size 100 khi gọi hàm size()
Mockito.when(mockedList.size()).thenReturn(100);
// Throw exception khi gọi hàm size()
Mockito.when(mockedList.size()).thenThrow(NullPointerException.class);
// Bạn có thể đổi ngược cách viết, còn logic vẫn vậy
// Ném ra exception khi gọi hàm .get() với tham số truyền vào bất kì
Mockito.doThrow(NullPointerException.class).when(mockedList.get(Mockito.any()));
```

### **Spy**

`Spy` là việc sửa một đối tượng Thật -> Giả

```
@RunWith(MockitoJUnitRunner.class)
public class SpyTest {
    @Spy
    List<String> list = new ArrayList<>();

    @Test
    public void testSpy() {
        list.add("one");
        list.add("two");
        // show the list items
        System.out.println(list);

        Mockito.verify(list).add("one");
        Mockito.verify(list).add("two");

        // @Spy thực sự gọi hàm .add của List nên nó có size là 2 mà không cần giả lập
        Assert.assertEquals(2, list.size());

        // Vẫn có thể làm giả thông tin gọi hàm với @Spy
        Mockito.when(list.size()).thenReturn(100);

        Assert.assertEquals(100, list.size());
    }
}
```

### **Captor**

`Captor` có nhiệm vụ ghi lại các tham số của đối tượng

```
@RunWith(MockitoJUnitRunner.class)
public class CaptorTest {
    @Mock
    List<Object> list;

    @Captor
    ArgumentCaptor<Object> captor;

    @Test
    public void testCaptor1() {
        list.add(1);
        // Capture lần gọi add vừa rồi có giá trị là gì
        Mockito.verify(list).add(captor.capture());

        System.out.println(captor.getAllValues());

        Assert.assertEquals(1, captor.getValue());
    }
}
```

### **Inject Mock**

Quay lại với ví dụ đầu bài viết:

Tôi có một lớp `Service` chứa lớp `DatabaseDriver` như sau:

```
public interface DatabaseDriver {
    List<Object> get() throws SQLException;
}

@Data
@AllArgsConstructor
public static class SuperService {
    DatabaseDriver driver;

    public List<Object> getObjects() throws SQLException {
        System.out.println("LOG: Getting objects");
        List<Object> list = driver.get();

        System.out.println("LOG: Sorting");
        Collections.sort(list, Comparator.comparingInt(value -> Integer.valueOf(value.toString())));

        System.out.println("LOG: Done");
        return list;
    }
}
```

Rõ ràng thì `driver` chính là mắt xích trong trong việc `SuperService` có hoạt động được hay không. Vì thế, muốn test được `SuperService`, chúng ta phải mock đối tượng `driver`.

kịch bản bây giờ sẽ là tôi mock một đối tượng của `DatabaseDriver` rồi truyền nó vào đối tượng `SuperService`.

Để truyền một đối tượng mock vào một đối tượng khác, chúng ta dùng `@InjectMocks`.

```
@RunWith(MockitoJUnitRunner.class)
public class InjectMockTest {
    @Mock
    DatabaseDriver driver;

    /**
     * Inject driver vào superService.
     * Mọi người có thể liên tưởng nó giống với Spring Injection
     */
    @InjectMocks
    SuperService superService;

    @Test(expected = SQLException.class)
    public void testInjectMock() throws SQLException {
        // Giả lập cho driver luôn trả về list (3,2,1) khi được gọi tới
        Mockito.doReturn(Arrays.asList(3, 2, 1)).when(driver).get();

        Assert.assertEquals(driver, superService.getDriver());

        // Test xem superService trả ra ngoài có đúng không?
        Assert.assertEquals(Arrays.asList(1, 2, 3), superService.getObjects());

        // Giả lập cho driver bắn exception
        Mockito.when(driver.get()).thenThrow(SQLException.class);
        superService.getObjects();
    }
}
```

### **Kết**

Tới đây, bạn đã có thể dùng `Mockito` xoành xoạch rồi.

Tiếp theo, có thể đọc cách sử dụng `Mockito` với `Spring Boot`tại đây

💁 Nếu có, toàn bộ project / code mẫu được lưu trữ tại **GitHub**

🌟 Đây là một bài viết trong Series **Làm chủ Spring Boot – Zero to Hero**

[SB21] Hướng dẫn tạo Bean có điều kiện với @Conditional
======================================================================

- Giới thiệu
- Cách tạo bean có điều kiện
- @ConditionalOnBean
- @ConditionalOnProperty
- @ConditionalOnExpression
- @ConditionalOnMissingBean
- @ConditionalOnResource
- @ConditionalOnClass
- @ConditionalOnMissingClass
- @ConditionalOnJava
- Một số loại khác
- Custom Conditional
- Kết

### **Giới thiệu**

Khi xây dựng một chương trình với **Spring Boot** đôi lúc chúng ta một **Bean** chỉ được load lên hoặc khởi tạo theo một điều kiện nào đó. Ví dụ như tạo một **Bean** trong môi trường Test, còn môi trường thật sẽ không cần nữa.

**Spring Boot** hỗ trợ chúng ta làm điều này với Annotation `@Conditional`.

Bài này yêu cầu kiến thức cơ bản:

1. 「Spring Boot #0」 Series làm chủ Spring Boot, Zero to Hero

Toàn bộ ví dụ trong bài viết này đều có tại Github

### **Cách tạo bean có điều kiện**

Trong **Spring Boot**, có rất nhiều cách để tạo ra `Bean`, bạn biết cách nào?

`@Component`, `@Configuration`, `@Bean`, `@Service`, v.v...

Với tất cả các cách tạo ra `Bean`, bạn đều có thể thêm **một hoặc nhiều điều kiện** để cho việc tạo ra `Bean` đó chỉ xảy ra nếu nó thỏa mãn một điều kiện cho trước.

**SpringBoot** hỗ trợ rất nhiều loại điều kiện khác nhau, tất cả đều sẽ bắt đầu bằng tiền tố `@Conditional...`. Còn hậu tố thì chúng ta sẽ đề cập sau.

Cách thức hoạt động của tất cả `@Condition` như sau:

```
@Configuration
public class ConditionalOnBeanExample {
    /*
    ABeanWithCondition chỉ được tạo ra khi @Condition thỏa mãn
    */
    @Bean
    @Conditional...
    ABeanWithCondition aBeanWithACondition() {
        return new ABeanWithCondition();
    }
}
```

Nếu bạn gắn nó trên một `@Configuration` thì toàn bộ các `@Bean` bên trong sẽ bị chịu tác động.

```
@Conditional...
@Configuration
public class ConditionalOnBeanExample {
    /*
    SomeOtherBean chỉ được tạo ra khi @Condition thỏa mãn
    */
    @Bean
    SomeOtherBean someOtherBean() {
        return new SomeOtherBean();
    }
       /*
    SomeOtherBean2 chỉ được tạo ra khi @Condition thỏa mãn
    */
    @Bean
    SomeOtherBean2 someOtherBean2() {
        return new SomeOtherBean2();
    }
}
```

Tương tự cho tất cả các annotation khác như `@Component`, `@Service`, `@Repository`

```
@Conditional...
@Component
public class ConditionalOnBeanExample {
}
```

Bây giờ chúng ta sẽ đi tìm hiểu các loại điều kiện trong **Spring Boot**.

### **@ConditionalOnBean**

`@ConditionalOnBean` sử dụng khi chúng ta muốn tạo ra một Bean, chỉ khi có một Bean khác đang tồn tại

```
@Configuration
public class ConditionalOnBeanExample {
    /*
    Đây là một Bean, bạn hãy chạy ứng dụng
    khi comment và chạy lại lần nữa nhưng bỏ dấu comment phía
    dưới để xem kết quả.
     */
    // @Bean
    RandomBean randomBean() {
        return new RandomBean();
    }

    /*
    ABeanWithCondition chỉ được tạo ra, khi RandomBean tồn tại trong Context.
     */
    @Bean
    @ConditionalOnBean(RandomBean.class)
    ABeanWithCondition aBeanWithACondition() {
        return new ABeanWithCondition();
    }
}
```

### **@ConditionalOnProperty**

Dùng `@ConditionalOnProperty` khi bạn muốn quyết định sự tồn tại Bean thông qua cấu hình property.

```
@Configuration
public class ConditionalOnPropertyExample {

    /*
    @ConditionalOnProperty giúp gắn điều kiện cho @Bean dựa theo
    property trong config
     */
    @Bean
    @ConditionalOnProperty(
            value="loda.bean2.enabled",
            havingValue = "true", // Nếu giá trị loda.bean2.enabled  = true thì Bean mới được khởi tạo
            matchIfMissing = false) // matchIFMissing là giá trị mặc định nếu không tìm thấy property loda.bean2.enabled
    ABeanWithCondition2 aBeanWithCondition2(){
        return new ABeanWithCondition2();
    }
}
```

_application.properties_

```
#Thay đổi thành true để tạo bean2
loda.bean2.enabled=true
```

### **@ConditionalOnExpression**

Trong trường hợp bạn muốn thỏa mãn nhiều điều kiện trong property, hãy sử dụng `@ConditionalOnExpression`

```
@Configuration
@ConditionalOnExpression(
        "${loda.expression1.enabled:true} and ${loda.expression2.enabled:true}"
)
public class ConditionalOnExpressionExample {
}
```

### **@ConditionalOnMissingBean**

Nếu trong `Context` chưa tồn tại bất kỳ Bean nào tương tự, thì `@ConditionalOnMissingBean` sẽ thỏa mãn điều kiện và tạo ra một Bean như thế.

```
@Configuration
public class ConditionalOnMissingBeanExample {
    /**
     * Nếu trong Context chưa tồn tại một SomeMissingBean nào
     * Thì Bean ở dưới đây mới được tạo
     * @return
     */
    @ConditionalOnMissingBean
    DataSource someMissingBean(){
        return new DataSource();
    }
}
```

### **@ConditionalOnResource**

`@ConditionalOnResource` thỏa mãn khi có một resources nào đó do bạn chỉ định tồn tại

```
/*
Nếu Spring Boot không tìm thấy file application.properties thì class này không được tạo
*/
@Configuration
@ConditionalOnResource(resources = "/application.properties")
public class ConditionalOnResourceExample {
}
```

### **@ConditionalOnClass**

`@ConditionalOnClass` thỏa mãn khi trong classpath có tồn tại class mà bạn yêu cầu

```
@Configuration
@ConditionalOnClass(name = "loda.me.spring.LodaHandsome")
class ConditionalOnClassExample {
}
```

### **@ConditionalOnMissingClass**

`@ConditionalOnMissingClass` ngược lại với `@ConditionalOnClass`. thỏa mãn khi trong classpath **không** tồn tại class mà bạn yêu cầu

```
@Configuration
@ConditionalOnMissingClass(name = "loda.me.spring.LodaHandsome")
class ConditionalOnMissingClassExample {
}
```

### **@ConditionalOnJava**

`@ConditionalOnJava` thỏa mãn nếu môi trường chạy version Java đúng với điều kiện

```
@Configuration
@ConditionalOnJava(JavaVersion.EIGHT)
class ConditionalOnJavaExample {
}
```

### **Một số loại khác**

Vẫn còn các bạn ạ, những mình thấy nó ít khi được sử dụng, nên sẽ không đề cập, tránh loạn thông tin.

### **Custom Conditional**

Tất nhiên là bạn hoàn toàn có thể tự tạo ra cho mình một điều kiện. Mình sẽ đề cập cách làm ở bài sau.

### **Kết**

💁 Nếu có, toàn bộ project / code mẫu được lưu trữ tại **GitHub**

🌟 Đây là một bài viết trong Series **Làm chủ Spring Boot – Zero to Hero**

[SB22] Hướng dẫn tự tạo custom @Conditional
====================================================

- Giới thiệu
- Tự tạo @Conditional
- Tự tạo Annotation @Conditional
- Chạy thử
- Kết hợp nhiều điều kiện với OR
- Kết hợp nhiều điều kiện với AND
- Kết

### **Giới thiệu**

Yêu cầu bạn phải đọc bài viết về `@Conditional` trước:

1. [Spring Boot] Hướng dẫn tạo Bean có điều kiện với @Conditional

Tôi đã giới thiệu với các bạn các sử dụng các loại `@Conditional` có sẵn trong Spring Boot. Tuy nhiên, trên thực tế, sẽ có những lúc yêu cầu các loại điều kiện nằm ngoài phạm vi của Spring Boot cung cấp.

Khi đó, chúng ta phải tự tạo `@Condinal` cho mình.

### **Tự tạo @Conditional**

Để tạo ra một điều kiện cho mình, bạn cần kế thừa lớp `Condition`, và implement lại function `matches`

`matches` là nơi bạn kiểm tra điều kiện xem có thoả mãn không.

```
/*
Một điều kiện, phải kế thừa lớp Condition của Spring Boot cung cấp
 */
public class WindowRequired implements Condition{

    @Override
    public boolean matches(ConditionContext conditionContext, AnnotatedTypeMetadata annotatedTypeMetadata) {
        // Nếu OS ra window trả ra true.
        return System.getProperty("os.name").toLowerCase().contains("win");
    }
}
```

Và khi đã định nghĩa được cho mình một điều kiện riêng, bạn có thể sử dụng như sau:

```
@Configuration
public class AppConfiguration {
    public static class SomeBean{
    }

    /*
    SomeBean chỉ được tạo ra khi
    thỏa mãn điều kiện
     */
    @Conditional(WindowRequired.class)
    @Bean
    SomeBean someBean(){
        return new SomeBean();
    }
}
```

### **Tự tạo Annotation @Conditional**

Nếu việc viết `@Conditional(WindowRequired.class)` chưa làm bạn hài lòng, bạn có thể tự tạo ra một `Annotation` giống với Spring Boot.

Ví dụ như `@ConditionalOnClass` trong bài viết trước

Thì hãy làm như sau:

```
@Target({ ElementType.TYPE, ElementType.METHOD })
@Retention(RetentionPolicy.RUNTIME)
@Documented
// Đánh dấu annotation này bởi @Conditional(WindowRequired.class)
@Conditional(WindowRequired.class)
public @interface ConditionalOnWindow {
    /*
    Trong trường hợp bạn muốn viết ngắn gọn,
    hay tạo ra 1 Annotation mới và gắn @Conditional(WindowRequired.class)
    trên nó

    Như vậy khi cần sử dụng chỉ cần gọi @ConditionalOnWindow là được
     */
}
```

Khi sử dụng:

```
@Configuration
public class AppConfiguration {
    public static class SomeBean{
    }

    /*
    SomeBean chỉ được tạo ra khi
    thỏa mãn điều kiện
     */
    @ConditionalOnWindow
    @Bean
    SomeBean someBean(){
        return new SomeBean();
    }
}
```

### **Chạy thử**

```
@SpringBootApplication
public class App {
    public static void main(String[] args) {
        ApplicationContext context = SpringApplication.run(App.class, args);
        try {
            SomeBean someBean = context.getBean(SomeBean.class);
            System.out.println("SomeBean tồn tại!");
        }catch (Exception e){
            System.out.println("SomeBean chỉ được tạo nếu chạy trên Window");
        }

    }
}
```

Hãy bỏ Annotation `@ConditionalOnWindow` đi và chạy thử, sau đó thêm lại, xem kết quả của 2 lần chạy.

### **Kết hợp nhiều điều kiện với OR**

Bạn có thể kết hợp nhiều điều kiện với nhau bởi phép OR.

Spring Boot hỗ trợ điều này bằng cách kế thừa lớp `AnyNestedCondition`

```
/**
 * Class kế thừa AnyNestedCondition sẽ chấp nhận mọi
 * điều kiện @Conditional bên trong nó
 */
public class WindowOrMacRequired extends AnyNestedCondition{

    public WindowOrMacRequired(){
        super(ConfigurationPhase.REGISTER_BEAN);
    }

    /*
    Bạn phải định nghĩa các Điều kiện bên trong class
    kế thừa AnyNestedCondition
     */
    @Conditional(WindowRequired.class)
    public class RunOnWindow{}

    /*
    Lúc này, cả 2 điều kiện Window và Mac sẽ được kết hợp vs
    nhau khi kiểm tra, nếu thoả mãn 1 trong 2 là đc
     */
    @Conditional(MacRequired.class)
    public class RunOnMac{}
}
```

Sử dụng

```
@Configuration
public class AppConfiguration {
    public static class SomeBean{
    }

    /*
    SomeBean chỉ được tạo ra khi
    thỏa mãn điều kiện
     */
    @Conditional(WindowOrMacRequired.class)
    @Bean
    SomeBean someBean(){
        return new SomeBean();
    }
}
```

### **Kết hợp nhiều điều kiện với AND**

Bạn có thể kết hợp các điều kiện bằng phép AND bằng cách kế thừa lớp `AllNestedConditions`.

Cách kế thừa của nó giống với `AnyNestedCondition` nên tôi sẽ không cần đề cập thêm.

Ngoài ra, có một cách khác là sử dụng nhiều custom `@Conditional` cùng một lúc.

```
@Bean
@ConditionalOnWindow
@Conditional(MacRequired.class)
SomeBean someBean() {
  return new SomeBean();
}
```

### **Kết**

Tới đây, bạn có thể nắm vững được cách tạo điều kiện cho cấu hình ứng dụng của mình, nó sẽ rất có ích khi bạn làm việc và tách biệt được hai môi trường dev và production.

💁 Nếu có, toàn bộ project / code mẫu được lưu trữ tại **GitHub**

🌟 Đây là một bài viết trong Series **Làm chủ Spring Boot – Zero to Hero**

[SB23] Xử lý sự kiện với @EventListener + @Async
=========================================================

- Giới thiệu
- Cơ bản về Event & Listener
- Áp vào thực tế
- Event
- Event Publisher
- Event Listener
- Chạy thử 1
- @Async
- @Async
- Chạy thử lần 2
- Kết

### **Giới thiệu**

Hẳn trong **Java** không ít thì nhiều các bạn đã từng sử dụng qua `Listener Pattern` rồi, nên tôi nghĩ sẽ không giới thiệu về nó nữa.

Trong bài viết này, chúng ta tập trung vào cách làm sao để thực hiện việc tạo ra `Event` và xử lý `Event` đó một cách gọn gàng với **Spring Boot**.

Bài này yêu cầu kiến thức cơ bản:

1. 「Spring Boot #0」 Series làm chủ Spring Boot, Zero to Hero

### **Cơ bản về Event & Listener**

!image

Cơ bản là khi chương trình của bạn đang vận hành, và có một công việc gì đó, bạn không muốn xử lý trực tiếp tại Class hiện hành hoặc muốn thông báo cho các Đối tượng khác biết bạn vừa làm gì.

Thì bạn sẽ bắn ra một object gọi là `Event` (sự kiện), có hoặc không thông tin đi kèm, và nhiệm vụ của các thằng khác là đón lấy hay lắng nghe sự kiện đó để xử lý nghiệp vụ của riêng nó, thằng xử lý gọi là `Listener`.

Thằng gây ra sự kiện gọi là `Source`.

Còn thằng cầm sự kiện đó ném cho `Listener` gọi là `Pushlisher`

### **Áp vào thực tế**

Giả sử bạn có một cái chuông cửa, khi có người tới bấm cái chuông này. Chuông sẽ phát ra tiếng kêu.

Ở trong nhà có chó, nó nghe thấy tiếng kêu, nó sẽ sủa lên.

Thì:

- `Source`: Người bấm chuông cửa, là người gây ra sự kiện.
- `Event`: sự kiện bấm chuông cửa
- `Pushlisher`: Cái chuông phát ra âm thanh (sự kiện) để thông báo.
- `Listener`: Con chó lắng nghe và xử lý sự kiện

### **Event**

Một `Event` (sự kiện) muốn được **Spring Boot** hỗ trợ thì sẽ phải kế thừa lớp `ApplicationEvent`.

```
/*
DoorBellEvent phải kế thừa lớp ApplicationEvent của Spring
Như vậy nó mới được coi là một sự kiện hợp lệ.
 */
public class DoorBellEvent extends ApplicationEvent {
    /*
        Mọi Class kế thừa ApplicationEvent sẽ
        phải gọi Constructor tới lớp cha.
     */
    public DoorBellEvent(Object source, String guestName) {
        // Object source là object tham chiếu tới
        // nơi đã phát ra event này!
        super(source);
    }
}
```

### **Event Publisher**

Trong **Spring Boot**, để bắn ra một sự kiện chúng ta sử dụng đối tượng `ApplicationEventPublisher`. Đây là một `Bean` có sẵn trong `Context` do Spring cung cấp, bạn chỉ cần lôi ra sử dụng thôi.

```
@Component
public class MyHouse {
    @Autowired
    ApplicationEventPublisher applicationEventPublisher;

    /**
     * Hành động bấm chuông cửa
     */
    public void rangDoorbellBy(String guestName) {
        // Phát ra một sự kiện DoorBellEvent
        // source (Nguồn phát ra) chính là class này
        applicationEventPublisher.publishEvent(new DoorBellEvent(this, guestName));
    }
}
```

### **Event Listener**

Để lắng nghe các sự kiện do `ApplicationEventPublisher` bắn ra, chúng ta sử dụng `@EventListener`

```
// Tạo ra Bean
@Component
public class MyDog {

    /*
        @EventListener sẽ lắng nghe mọi sự kiện xảy ra
        Nếu có một sự kiện DoorBellEvent được bắn ra, nó sẽ đón lấy
        và đưa vào hàm để xử lý
     */
    @EventListener
    public void doorBellEventListener(DoorBellEvent doorBellEvent) throws InterruptedException {
        // Giả sử con chó đang ngủ, 1 giây sau mới dậy
        Thread.sleep(1000);
        // Sự kiện DoorBellEvent được lắng nghe và xử lý tại đây
        System.out.println(Thread.currentThread().getName() + ": Chó ngủ dậy!!!");
        System.out.println(String.format("%s: Go go!! Có người tên là %s gõ cửa!!!", Thread.currentThread().getName(), doorBellEvent.getGuestName()));
    }
}
```

`@EventListener` gắn trên một method, với tham số đầu vào chính là sự kiện mà bạn muốn lắng nghe.

Lưu ý: Class chịu trách nhiệm xử lý, có chứa method `@EventListener` cũng phải là Bean nhé.

### **Chạy thử 1**

```
@SpringBootApplication
public class App {
    @Autowired
    MyHouse myHouse;

    public static void main(String[] args) {
        SpringApplication.run(App.class, args);
    }

    @Bean
    CommandLineRunner run() {
        return args -> {
            System.out.println(Thread.currentThread().getName() + ": Loda đi tới cửa nhà !!!");
            System.out.println(Thread.currentThread().getName() + ": => Loda bấm chuông và khai báo họ tên!");
            // gõ cửa
            myHouse.rangDoorbellBy("Loda");
            System.out.println(Thread.currentThread().getName() +": Loda quay lưng bỏ đi");
        };
    }

}
```

OUTPUT:

```
restartedMain: Loda đi tới cửa nhà !!!
restartedMain: => Loda bấm chuông và khai báo họ tên!
restartedMain: Chó ngủ dậy!!!
restartedMain: Go go!! Có người tên là Loda gõ cửa!!!
restartedMain: Loda quay lưng bỏ đi
```

Bạn sẽ thấy quá trình xử lý ở đây xảy ra một cách **tuần tự** và đồng bộ (Synchronous).

Vậy là chúng ta có thể hiểu:

> nếu không cấu hình gì thêm, ApplicationEvent và @EventListener là Synchronous.

Chương trình sẽ phải chờ sự kiện xử lý xong thì mới được chạy tiếp.

### **@Async**

Đa phần, xử lý Synchronous không phải điều mà chúng ta mong đợi, chúng ta muốn việc xử lý sự kiện có thể hoạt động riêng và không ảnh hưởng tới luồng làm việc chính.

Nói cách khác, chúng ta muốn sự kiện được xử lý ở một Thread khác, đây gọi là **bất đồng bộ (Asynchronous)**

Để làm được điều này, chúng ta cần kích hoạt chức năng xử lý bất đồng bộ của **Spring Boot**, bằng cách bổ sung annotation `@EnableAsync`.

```
@Configuration
@EnableAsync
public class ListenerConfiguration {
    /**
     * Tạo ra Executor cho Async
     * @return
     */
    @Bean
    TaskExecutor taskExecutor() {
        return new SimpleAsyncTaskExecutor();
    }
}
```

**Spring Boot** khi thấy Annotation này, sẽ kích hoạt cho phép xử lý sự kiện dưới dạng Async

các `Event` sẽ được gửi vào một `Executor` (đơn giản nhất là `SimpleAsyncTaskExecutor`) và chờ được xử lý.

Nếu chưa biết `Executor` là gì, bạn có thể đọc 2 bài viết sau:

1. Khái niệm ThreadPool và Executor trong Java
2. ThreadPoolExecutor và nguyên tắc quản lý pool size

### **@Async**

Sau khi kích hoạt tính năng `Async`, bất kỳ sự kiện nào bạn muốn nó xử lý Async thì hãy đánh dấu nó bởi `@Async`.

```
@Component
public class MyDog {

    /*
        @EventListener sẽ lắng nghe mọi sự kiện xảy ra
        Nếu có một sự kiện DoorBellEvent được bắn ra, nó sẽ đón lấy
        và đưa vào hàm để xử lý
     */
    /*
        @Async là cách lắng nghe sự kiện ở một Thread khác, không ảnh hưởng tới
        luồng chính
     */
    @Async
    @EventListener
    public void doorBellEventListener(DoorBellEvent doorBellEvent) throws InterruptedException {
        // Giả sử con chó đang ngủ, 1 giây sau mới dậy
        Thread.sleep(1000);
        // Sự kiện DoorBellEvent được lắng nghe và xử lý tại đây
        System.out.println("Chó ngủ dậy!!!");
        System.out.println(String.format("Go go!! Có người tên là %s gõ cửa!!!", doorBellEvent.getGuestName()));
    }
}
```

### **Chạy thử lần 2**

OUTPUT:

```
restartedMain: Loda đi tới cửa nhà !!!
restartedMain: => Loda bấm chuông và khai báo họ tên!
restartedMain: Loda quay lưng bỏ đi
SimpleAsyncTaskExecutor-1: Chó ngủ dậy!!!
SimpleAsyncTaskExecutor-1: Go go!! Có người tên là Loda gõ cửa!!!
```

Lần này quá trình xử lý đã diễn ra đúng như chúng ta mong đợi, người bấm cứ bấm, mà chó kêu cứ kêu, mỗi người một việc, chả ai ảnh hưởng tới ai, chỉ cần biết có sự kiện xảy ra thì phản ứng là được.

### **Kết**

💁 Nếu có, toàn bộ project / code mẫu được lưu trữ tại **GitHub**

🌟 Đây là một bài viết trong Series **Làm chủ Spring Boot – Zero to Hero**

[SB24] RESTful API Document Tạo với Spring Boot + Swagger
=============================================================

- Giới thiệu
- cài đặt
- Tổng quan Swagger
- Prepare
- Config Swagger
- Góc custom

### **Giới thiệu**

**Spring Boot** hỗ trợ chúng ta tạo ra RESTful API một cách nhanh chóng và tiện lợi, giúp sản phẩm được vận hành nhanh nhất có thể.

Tuy nhiên, Việc deploy nhanh chóng một services không đồng nghĩa với việc nó có thể sử dụng được. Thông thường, tất cả các API sau khi được đưa lên sẽ phải đi kèm với document mô tả, để bất kì ai sử dụng đến thì có thể tra cứu.

Thật không may là việc làm document chưa bao giờ là dễ dàng cả :(( từ lí do này, `Swagger` ra đời để giúp chúng ta mô tả tài liệu dự án một cách nhanh chóng bằng annotation.

Trong bài có đề cập các kiến thức:

1. Spring Boot
2. jpa
3. lombok

### **cài đặt**

Trong bài này, tôi sẽ hướng dẫn các bạn sử dụng `Swagger2` và tuân theo các quy tắc cảu **Swagger Specification 2.0** nhé.

Tại thời điểm viết bài này, hiện phiên bản mới nhất là 3, tuy nhiên, nó sẽ là **OpenAPI 3.0**.

_pom.xml_

```
<?xml version="1.0" encoding="UTF-8"?>
<projectxmlns="http://maven.apache.org/POM/4.0.0"xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"xsi:schemaLocation="http://maven.apache.org/POM/4.0.0https://maven.apache.org/xsd/maven-4.0.0.xsd"><modelVersion>4.0.0</modelVersion><parent><groupId>org.springframework.boot</groupId><artifactId>spring-boot-starter-parent</artifactId><version>2.0.5.RELEASE</version><relativePath/> <!-- lookup parent from repository -->
	</parent><groupId>me.loda.spring</groupId><artifactId>example-independent-maven-spring-project</artifactId><version>0.0.1-SNAPSHOT</version><name>example-independent-maven-spring-project</name><description>Demo project for Spring Boot</description><properties><java.version>1.8</java.version></properties><dependencies><dependency><groupId>org.springframework.boot</groupId><artifactId>spring-boot-starter-web</artifactId></dependency><dependency><groupId>org.springframework.boot</groupId><artifactId>spring-boot-devtools</artifactId><scope>runtime</scope><optional>true</optional></dependency><dependency><groupId>org.projectlombok</groupId><artifactId>lombok</artifactId><optional>true</optional></dependency><dependency><groupId>org.springframework.boot</groupId><artifactId>spring-boot-starter-test</artifactId><scope>test</scope></dependency>

		<dependency><groupId>io.springfox</groupId><artifactId>springfox-swagger2</artifactId><version>2.9.2</version></dependency><dependency><groupId>io.springfox</groupId><artifactId>springfox-swagger-ui</artifactId><version>2.9.2</version></dependency><!--spring jpa-->
		<dependency><groupId>org.springframework.boot</groupId><artifactId>spring-boot-starter-data-jpa</artifactId></dependency><!--in memory database-->
		<dependency><groupId>com.h2database</groupId><artifactId>h2</artifactId><scope>runtime</scope></dependency></dependencies><build><plugins><plugin><groupId>org.springframework.boot</groupId><artifactId>spring-boot-maven-plugin</artifactId></plugin></plugins></build></project>
```

`springfox` là một thư viện java implementation của Swagger Specification.

`springfox-swagger2` chứa core của swagger, giúp chúng ta khai báo document cho api.

`springfox-swagger-ui` giúp chúng ta biểu diễn tài liệu dưới dạng web view, dễ nhìn và test.

### **Tổng quan Swagger**

Để sử dụng cơ bản thì `Swagger` cung cấp một số các Annotations hữu ích sau:

Chúng ta đi vào thực hành thử nhé.

Đại loại sau khi làm xong, chúng ta sẽ có 1 web view document như thế này:

!image

### **Prepare**

Tạo ra class Model

_User.java_

```
@Data
@Entity
@Table
@NoArgsConstructor
@AllArgsConstructor
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String firstName;
    private String lastName;
    private String email;
}
```

_UserRepository.java_

```
@Repository
public interface UserRepository extends JpaRepository<User, Long> {

}
```

Khi đã có được Model và Repository, chúng ta sẽ tạo Controller để thao tác liên quan tới `User` nhé.

```
@RestController
@RequestMapping("/api/v1")
@RequiredArgsConstructor
public class UserController {
    private final UserRepository userRepository;

    @GetMapping("/users")
    public List<User> getAllUsers() {
        return userRepository.findAll();
    }

    @GetMapping("/users/{id}")
    public User getUser(@PathVariable("id") Long id) {
        return userRepository.findById(id).orElse(new User());
    }

    @PostMapping("/users")
    public User createUser(@Valid @RequestBody User user) {
        return userRepository.save(user);
    }

    @PutMapping("/users/{id}")
    public User updateUser(@PathVariable("id") Long id, @Valid @RequestBody User user) {
        user.setId(id);
        return userRepository.save(user);
    }

    @DeleteMapping("/users/{id}")
    public void deleteUser(@PathVariable("id") Long id) {
        userRepository.deleteById(id);
    }
}
```

Âu khê, tạm thời như thế đã 😂

Bây giờ vào phần chính, config `Swagger` cho dự án của chúng ta.

### **Config Swagger**

Thật may mắn, trước khi đi vào các custom phức tạp thì Swagger hỗ trợ chúng ta tự động sinh ra tài liệu một cách mặc định mà chưa cần phải khai báo bất kì annotation nào đã giới thiệu ở trên.

Chỉ cần tạo ra đối tượng `Docket` của `Swagger` và nó sẽ quét hết các địa chỉ API mà bạn chỉ định, rồi tự động sinh ra tài liệu cơ bản cho chúng ta.

```
@Configuration
@EnableSwagger2
public class Swagger2Config {
    @Bean
    public Docket api() {
        return new Docket(DocumentationType.SWAGGER_2).select()
                                                      .apis(RequestHandlerSelectors.basePackage("me.loda.spring.swagger.controller"))
                                                      .paths(PathSelectors.regex("/.*"))
                                                      .build()
                                                      .apiInfo(apiEndPointsInfo());
    }

    private ApiInfo apiEndPointsInfo() {
        return new ApiInfoBuilder().title("Spring Boot REST API")
                                   .description("Employee Management REST API")
                                   .contact(new Contact("loda", "https://loda.me/", "loda.namnh@gmail.com"))
                                   .license("Apache 2.0")
                                   .licenseUrl("http://www.apache.org/licenses/LICENSE-2.0.html")
                                   .version("1.0.0")
                                   .build();
    }
}
```

Các thứ cần lưu ý bao gồm:

1. Để `Swagger` hoạt động, bạn nhớ kích hoạt nó bằng `@EnableSwagger2`.
2. Bạn có thể chọn nơi chứa các API bằng `RequestHandlerSelectors`. Nếu muốn quét hết cả project, có thể xài `RequestHandlerSelectors.any()`
3. Bạn có thể chỉ định bộ lọc cho các api bằng `PathSelectors`. Nếu muốn quét tất cả, chọn `PathSelectors.any()`.

```
@SpringBootApplication
public class App {
    public static void main(String[] args) {
        SpringApplication.run(App.class, args);
    }
}
```

Bây giờ, chạy thử và vào địa chỉ `http://localhost:8080/swagger-ui.html` để xem thành quả nhé.

### **Góc custom**

Bạn có thể chỉ định rõ hơn các mô tả của tài liệu bằng cách sử dụng các Annotation mà `Swagger` cung cấp.

_User.java_

```
@Data
@Entity
@Table
@NoArgsConstructor
@AllArgsConstructor
@ApiModel(value = "User model")
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    @ApiModelProperty(notes = "The database generated User ID")
    private Long id;

    private String firstName;
    private String lastName;
    private String email;
}
```

_UserController.java_

```
@RestController
@RequestMapping("/api/v1")
@RequiredArgsConstructor
@Api(value = "User APIs")
public class UserController {
    private final UserRepository userRepository;

    @ApiOperation(value = "Xem danh sách User", response = List.class)
    @ApiResponses(value = {
            @ApiResponse(code = 200, message = "Thành công"),
            @ApiResponse(code = 401, message = "Chưa xác thực"),
            @ApiResponse(code = 403, message = "Truy cập bị cấm"),
            @ApiResponse(code = 404, message = "Không tìm thấy")
    })
    @GetMapping("/users")
    public List<User> getAllUsers() {
        return userRepository.findAll();
    }

    @GetMapping("/users/{id}")
    public User getUser(@PathVariable("id") Long id) {
        return userRepository.findById(id).orElse(new User());
    }

    @PostMapping("/users")
    public User createUser(
            @ApiParam(value = "Đối tượng User cần tạo mới", required = true) @Valid @RequestBody User user
    ) {
        return userRepository.save(user);
    }

    @PutMapping("/users/{id}")
    public User updateUser(@PathVariable("id") Long id, @Valid @RequestBody User user) {
        user.setId(id);
        return userRepository.save(user);
    }

    @DeleteMapping("/users/{id}")
    public void deleteUser(@PathVariable("id") Long id) {
        userRepository.deleteById(id);
    }
}
```

Tới đây bạn có thể sử dụng Swagger thoải mái rồi, tôi sẽ làm một bài khác về **OpenAPI 3.0**.

💁 Nếu có, toàn bộ project / code mẫu được lưu trữ tại **GitHub**

🌟 Đây là một bài viết trong Series **Làm chủ Spring Boot – Zero to Hero**

[SB25] RESTful API Document với Spring Boot + OpenApi 3.0
===========================================================

- Giới thiệu
- cài đặt
- Tổng quan OpenApi 3.0
- Prepare
- Config OpenApi 3.0
- Kết

### **Giới thiệu**

Trong bài viết trước:

1. RESTful API Document Tạo với Spring Boot + Swagger

Tôi đã giới thiệu lí do vì sao cần Document, và cách tạo ra nó nhanh chóng với `Swagger 2`.

Trong bài này, tôi sẽ giới thiệu thêm phiên bản tiếp theo, một tiêu chuẩn mới nhất về RESTful document đó là OpenApi 3.0.

Tại sao k đặt tên là `Swagger 3`? câu chuyện đằng sau nó là việc SmartBear mua lại Swagger, và đổi tên Swagger Specification thành OpenApi Specification, bắt đầu tạo ra các tiêu chuẩn mới cho xây dựng Document, tuy nhiên vẫn xây dựng trên Swagger core.

`OpenApi 3.0` là bản mới nhất được ra mắt tại thời điểm viết bài này.

Trong bài có đề cập các kiến thức:

1. Spring Boot
2. jpa
3. lombok

### **cài đặt**

_pom.xml_

```
<?xml version="1.0" encoding="UTF-8"?>
<projectxmlns="http://maven.apache.org/POM/4.0.0"xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"xsi:schemaLocation="http://maven.apache.org/POM/4.0.0https://maven.apache.org/xsd/maven-4.0.0.xsd"><modelVersion>4.0.0</modelVersion><parent><groupId>org.springframework.boot</groupId><artifactId>spring-boot-starter-parent</artifactId><version>2.0.5.RELEASE</version><relativePath/> <!-- lookup parent from repository -->
	</parent><groupId>me.loda.spring</groupId><artifactId>example-independent-maven-spring-project</artifactId><version>0.0.1-SNAPSHOT</version><name>example-independent-maven-spring-project</name><description>Demo project for Spring Boot</description><properties><java.version>1.8</java.version></properties><dependencies><dependency><groupId>org.springframework.boot</groupId><artifactId>spring-boot-starter-web</artifactId></dependency><dependency><groupId>org.springframework.boot</groupId><artifactId>spring-boot-devtools</artifactId><scope>runtime</scope><optional>true</optional></dependency><dependency><groupId>org.projectlombok</groupId><artifactId>lombok</artifactId><optional>true</optional></dependency><dependency><groupId>org.springframework.boot</groupId><artifactId>spring-boot-starter-test</artifactId><scope>test</scope></dependency><!--https://mvnrepository.com/artifact/org.springdoc/springdoc-openapi-core -->
        <dependency><groupId>org.springdoc</groupId><artifactId>springdoc-openapi-core</artifactId><version>1.1.49</version></dependency><dependency><groupId>org.springdoc</groupId><artifactId>springdoc-openapi-ui</artifactId><version>1.1.49</version></dependency><!--spring jpa-->
		<dependency><groupId>org.springframework.boot</groupId><artifactId>spring-boot-starter-data-jpa</artifactId></dependency><!--in memory database-->
		<dependency><groupId>com.h2database</groupId><artifactId>h2</artifactId><scope>runtime</scope></dependency></dependencies><build><plugins><plugin><groupId>org.springframework.boot</groupId><artifactId>spring-boot-maven-plugin</artifactId></plugin></plugins></build></project>
```

`springdoc` là một thư viện java implementation của OpenApi Specification 3.0.

`springdoc-openapi-core` chứa core của swagger, giúp chúng ta khai báo document cho api.

`springdoc-openapi-ui` giúp chúng ta biểu diễn tài liệu dưới dạng web view, dễ nhìn và test.

### **Tổng quan OpenApi 3.0**

`OpenApi 3.0` kế thừa và đổi mới khá nhiều các thành phần của `Swagger2`, khiến nó tường minh và dễ đọc hơn.

SWAGGER2OPENAPI 3.0DESCRIPTION@Api

@Tag

Đánh dấu 1 class là nơi chứa các API

@ApiModel

không còn

Đánh dấu 1 class là Swagger Model

@ApiModelProperty

@Schema

Bổ sung các thông tin cho

@ApiOperation

@Operation

Mô tả cho một API và response của nó

@ApiParam

@Parameter

Mô tả các parameter

@ApiResponse

@ApiResponse

Mô tả status code của response

@ApiResponses

@ApiResponses

Mô tả danh sách các status code của response

Chúng ta đi vào thực hành thử nhé.

Đại loại sau khi làm xong, chúng ta sẽ có 1 web view document như thế này:

!image

Ngoài đẹp hơn, mô tả chi tiết và dễ sử dụng hơn, thì có 1 phần rất hay, đó là chúng ta có thể chọn Server Url khi test API

!image

### **Prepare**

Tạo ra class Model

_User.java_

```
@Data
@Entity
@Table
@NoArgsConstructor
@AllArgsConstructor
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    @Schema(description = "User UUID in  the database")
    @JsonProperty("id")
    private Long id;

    private @NotBlank @Size(min = 0, max = 20) String firstName;
    private String lastName;

    private @NotBlank @Size(min = 0, max = 50) String email;
}
```

_UserRepository.java_

```
@Repository
public interface UserRepository extends JpaRepository<User, Long> {

}
```

Khi đã có được Model và Repository, chúng ta sẽ tạo Controller để thao tác liên quan tới `User` nhé.

Điểm khác biệt lớn nhất của `OpenApi3` so với `Swagger2` là hệ thống Annotation rất là dày đặc, đa phần các giá trị được biểu diễn và ăn khớp với nhau bằng Annotation và Class.

```
@RestController
@RequestMapping("/api/v1")
@RequiredArgsConstructor
@Tag(name = "user")
public class UserController {
    private final UserRepository userRepository;

    @Operation(description = "Xem danh sách User", responses = {
            @ApiResponse(content = @Content(array = @ArraySchema(schema = @Schema(implementation = User.class))), responseCode = "200") })
    @ApiResponses(value = {
            @ApiResponse(responseCode  = "200", description = "Thành công"),
            @ApiResponse(responseCode  = "401", description = "Chưa xác thực"),
            @ApiResponse(responseCode  = "403", description = "Truy cập bị cấm"),
            @ApiResponse(responseCode  = "404", description = "Không tìm thấy")
    })
    @GetMapping("/users")
    public List<User> getAllUsers() {
        return userRepository.findAll();
    }

    @GetMapping("/users/{id}")
    public User getUser(@PathVariable("id") Long id) {
        return userRepository.findById(id).orElse(new User());
    }

    @PostMapping("/users")
    public User createUser(
            @Valid
            @Parameter(description = "User model to create.", required = true, schema = @Schema(implementation = User.class))
            @RequestBody User user) {
        return userRepository.save(user);
    }

    @PutMapping("/users/{id}")
    public User updateUser(@PathVariable("id") Long id, @Valid @RequestBody User user) {
        user.setId(id);
        return userRepository.save(user);
    }

    @DeleteMapping("/users/{id}")
    public void deleteUser(@PathVariable("id") Long id) {
        userRepository.deleteById(id);
    }
}
```

Âu khê, Chỉ cần như vậy là chạy được rồi.

Nếu đầy đủ hơn, thì bạn nên config `OpenApi` cho dự án của chúng ta để bổ sung các thông tin tổng quan.

### **Config OpenApi 3.0**

Chỉ cần tạo ra đối tượng `OpenAPI` và cung cấp các thông tin cần thiết.

```
@Configuration
public class OpenApiConfig {
    @Bean
    public OpenAPI customOpenAPI() {
        return new OpenAPI()
                // Thiết lập các server dùng để test api
                .servers(Lists.newArrayList(
                        new Server().url("http://localhost:8080"),
                        new Server().url("https://user.loda.me")
                ))
                // info
                .info(new Info().title("Loda Application API")
                                .description("Sample OpenAPI 3.0")
                                .contact(new Contact()
                                                 .email("loda.namnh@gmail.com")
                                                 .name("loda")
                                                 .url("https://loda.me/"))
                                .license(new License()
                                                 .name("Apache 2.0")
                                                 .url("http://www.apache.org/licenses/LICENSE-2.0.html"))
                                .version("1.0.0"));
    }
}
```

`OpenApi3.0` thì bạn không cần `@Enable` mà chỉ cần add `springdoc-openapi-core` vào dependencies thôi là nó tự động gen ra tài liệu rồi.

```
@SpringBootApplication
public class App {
    public static void main(String[] args) {
        SpringApplication.run(App.class, args);
    }
}
```

Bây giờ, chạy thử và vào địa chỉ `http://localhost:8080/swagger-ui.html` để xem thành quả nhé.

### **Kết**

Tới đây bạn có thể sử dụng **OpenAPI 3.0** thoải mái rồi.

💁 Nếu có, toàn bộ project / code mẫu được lưu trữ tại **GitHub**

🌟 Đây là một bài viết trong Series **Làm chủ Spring Boot – Zero to Hero**

