[SS] Hướng dẫn Spring Security cơ bản, dễ hiểu
==========================================================

- Giới thiệu
- Cài đặt
- Implement
- Kích hoạt tính năng WebSecurity
- Tạo tài khoản user
- Phân quyền truy cập
- Thêm controller.
- Chạy thử
- Kết

### **Giới thiệu**

**Spring Security** là một trong những core feature quan trọng của Spring Framework, nó giúp chúng ta phân quyền và xác thực người dùng trước khi cho phép họ truy cập vào các tài nguyên của chúng ta.

Trong bài hướng dẫn này, tôi sẽ hướng dẫn các bạn cách implement **Spring Security** một cách cơ bản nhất, đơn giản nhất, chúng ta sẽ nâng cao dần ở các bài sau.

### **Cài đặt**

Chúng ta cài thư viện qua maven, các dependencies bao gồm:

_pom.xml_

```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://maven.apache.org/POM/4.0.0http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>org.springframework</groupId>
    <artifactId>spring-security-example</artifactId>
    <version>0.1.0</version>

    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.0.5.RELEASE</version>
    </parent>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-thymeleaf</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
        <dependency>
            <groupId>org.springframework.security</groupId>
            <artifactId>spring-security-test</artifactId>
            <scope>test</scope>
        </dependency>
    </dependencies>

    <properties>
        <java.version>1.8</java.version>
    </properties>

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

Thư mục code gồm có:

!image

### **Implement**

### **Kích hoạt tính năng WebSecurity**

Trước tiên, để kích hoạt tính năng Spring Security trên ứng dụng Web của mình, các bạn cần gắn annotation `@EnableWebSecurity` trên một bean bất kỳ của mình.

Ở đây, tôi tạo ra một class `WebSecurityConfig` để là nơi tập trung các xử lý các thông tin liên quan tới security.

```
@EnableWebSecurity
public class WebSecurityConfig {
    // ...
}
```

### **Tạo tài khoản user**

Thông thường, tài khoản user của người dùng sẽ được lưu trong csdl và mã hóa. Tuy nhiên, trong ví dụ cực kì basic này, tôi sẽ lưu một tài khoản người dùng trong chính bộ nhớ chương trình.

Cách này chỉ để demo thôi nhé, vì nó dữ liệu sẽ bị mất khi tắt ứng dụng, chúng ta sẽ tìm hiểu cách xác thực bằng csdl ở bài sau.

```
@EnableWebSecurity
public class WebSecurityConfig extends WebSecurityConfigurerAdapter {

    @Bean
    @Override
    public UserDetailsService userDetailsService() {
        // Tạo ra user trong bộ nhớ
        // lưu ý, chỉ sử dụng cách này để minh họa
        // Còn thực tế chúng ta sẽ kiểm tra user trong csdl
        InMemoryUserDetailsManager manager = new InMemoryUserDetailsManager();
        manager.createUser(
                User.withDefaultPasswordEncoder() // Sử dụng mã hóa password đơn giản
                    .username("loda")
                    .password("loda")
                    .roles("USER") // phân quyền là người dùng.
                    .build()
        );
        return manager;
    }
}
```

`WebSecurityConfigurerAdapter` là một interface tiện ích của Spring Security giúp chúng ta cài đặt các thông tin dễ dàng hơn.

_Method_`userDetailsService()` có tác dụng cung cấp thông tin user cho Spring Security, chúng ta _Override_ lại method này và cung cấp cho nó một `User` là `loda`.

### **Phân quyền truy cập**

Khi đã có `User`, chúng ta sẽ cần phân quyền xem một `User` sẽ được phép truy cập vào những tài nguyên nào.

Lúc này, vẫn ở trong `WebSecurityConfigurerAdapter`, chúng ta override lại method `protected void configure(HttpSecurity http)` để thực hiện việc phân quyền.

```
import org.springframework.context.annotation.Bean;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;
import org.springframework.security.core.userdetails.User;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.provisioning.InMemoryUserDetailsManager;

@EnableWebSecurity
public class WebSecurityConfig extends WebSecurityConfigurerAdapter {

    @Bean
    @Override
    public UserDetailsService userDetailsService() {
        // Tạo ra user trong bộ nhớ
        // lưu ý, chỉ sử dụng cách này để minh họa
        // Còn thực tế chúng ta sẽ kiểm tra user trong csdl
        InMemoryUserDetailsManager manager = new InMemoryUserDetailsManager();
        manager.createUser(
                User.withDefaultPasswordEncoder() // Sử dụng mã hóa password đơn giản
                    .username("loda")
                    .password("loda")
                    .roles("USER") // phân quyền là người dùng.
                    .build()
        );
        return manager;
    }

    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http
                .authorizeRequests()
                    .antMatchers("/", "/home").permitAll() // Cho phép tất cả mọi người truy cập vào 2 địa chỉ này
                    .anyRequest().authenticated() // Tất cả các request khác đều cần phải xác thực mới được truy cập
                    .and()
                .formLogin() // Cho phép người dùng xác thực bằng form login
                    .defaultSuccessUrl("/hello")
                    .permitAll() // Tất cả đều được truy cập vào địa chỉ này
                    .and()
                .logout() // Cho phép logout
                    .permitAll();
    }
}
```

`HttpSecurity` là đối tượng chính của Spring Security, cho phép chúng ta cấu hình mọi thứ cần bảo mật, và nó được xây dựng dưới design pattern giống với `Builder Pattern`, nên mọi cài đặt có thể viết liên tục thông qua toán tử `.`

Ở ví dụ trên, những gì chúng ta muốn cho phép, chúng ta sẽ xài method `.permit()`, còn những gì cấm hoặc yêu cầu xác thực sẽ dùng `.authenticated()`

Khi gọi `.formLogin()` thì chúng ta cấu hình cho phép người dùng đăng nhập, thông qua địa chỉ mặc định `/login` do Spring Security tự tạo ra (Cái này có thể custom theo ý mình được, nhưng chúng ta sẽ tiếp cận ở bài sau).

Tương tự `.logout()` cho phép người dùng logout, Nếu không nói gì thêm, Spring Security sẽ mặc định tự tạo ra một trang logout với địa chỉ `/logout`.

### **Thêm controller.**

Chúng ta tạo ra trang web của mình gồm có trang `homepage` và trang `hello`.

- Trang `homepage` thì ai cũng truy cập được.
- Trang `hello` thì phải xác thực mới được truy cập.

Các địa chỉ `/login` và `/logout` thì không cần tạo, Spring Security đã tạo sẵn rồi, chúng ta dùng lại luôn.

```
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;

@Controller
public class WebController {

    @GetMapping(value = {"/", "/home"})
    public String homepage() {
        return "home"; // Trả về home.html
    }

    @GetMapping("/hello")
    public String hello() {
        return "hello"; // Trả về hello.html
    }

}
```

Ukie, đơn giản vậy thôi, bây giờ cần tạo ra các trang `home.html` và `hello.html`.

Vì ở trong file _pom.xml_ chúng ta đã cài thư viện `thymeleaf`. Nên nó sẽ tự động mapping tên tương ứng trong `Controller` trả về với các file ở bên trong thư mục `resources/templates/`

```
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-thymeleaf</artifactId>
</dependency>
```

_hello.html_

```
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
  <title>Hello</title>
</head>
<body>
<h1>Chỉ những ai đăng nhập mới truy cập được trang này!</h1>

<form th:action="@{/logout}" method="post">
  <input type="submit" value="Sign Out"/>
</form>

</body>
</html>
```

Cú pháp `th:action="@{/logout}"` là của thymeleaf, có tác dụng request tới địa chỉ `/logout`

_home.html_

```
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
  <title>Spring Security Example</title>
</head>
<body>
<h1>Welcome!</h1>

<p><a href="/login">login</a></p>
<p>Click <a href="https://loda.me">here</a> to see a loda homepage.</p>

</body>
</html>
```

### **Chạy thử**

Chạy ứng dụng bằng cách implement `@SpringBootApplication`

```
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class App {
    public static void main(String[] args) {
        SpringApplication.run(App.class, args);
    }
}
```

Khi chạy xong, truy cập vào đường dẫn `http://localhost:8080/home` để vào trang chủ.

!image

Khi chưa đăng nhập, chúng ta truy cập vào đường dẫn `/hello`. Thì nó sẽ tự redirect sang trang `/login`.

!image

Khi đã đăng nhập thành công, chúng ta sẽ có thể vào trang `/hello` như bình thường.

!image

Khi click vào `Sign Out` thì sẽ đăng xuất.

!image

### **Kết**

Trong bài này, tôi đã giới thiệu với các bạn về **Spring Security** đồng thời giới thiệu các khái niệm cơ bản về User và phân quyền. Ở các bài nâng cao sau, tôi sẽ giới thiệu thêm với các bạn về cách xác thực người dùng trong csdl và xác thực bằng `OAuth 2.0`

Bài viết liên quan:

1. [⛳SS] Hướng dẫn Spring Security + Jpa Hibernate

💁 Nếu có, toàn bộ project / code mẫu được lưu trữ tại **GitHub**

🌟 Đây là một bài viết trong Series **Làm chủ Spring Boot – Zero to Hero**

[SS] Hướng dẫn Spring Security + Jpa Hibernate
===================================================

- Giới thiệu
- Cài đặt
- Implement
- Tạo User
- Tham chiếu User với UserDetails
- Cấu hình và phân quyền
- Tạo Controller và webpage
- Tạo thông tin User trong database
- # Chạy thử
- Kết

### **Giới thiệu**

Trong bài trước, tôi đã hướng dẫn với các bạn về cách để sử dụng **Spring Security** cơ bản, cách kích hoạt, tạo user và phân quyền.

Trong bài này, chúng sẽ tìm hiểu cách để kiểm tra một `User` ở bên trong csdl.

Trong bài viết giả định bạn đã có các kiến thức sau:

1. Hibernate
2. Lombok
3. Spring Security Cơ bản

### **Cài đặt**

Chúng ta sử dụng **Maven**, Cài đặt file _pom.xml_ của bạn như sau.

Khác với project trước đó, lần này chúng ta sử dụng thêm database `com.h2database`. Đây là một _in memory database_. Đại khái là mọi thứ bạn lưu trên database chỉ tồn tại trong bộ nhớ, khi tắt ứng dụng, database sẽ mất sạch, thích hợp cho việc demo.

```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://maven.apache.org/POM/4.0.0http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>org.springframework</groupId>
    <artifactId>spring-security-hibernate</artifactId>
    <version>0.1.0</version>

    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.0.5.RELEASE</version>
    </parent>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-thymeleaf</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>com.h2database</groupId>
            <artifactId>h2</artifactId>
            <scope>runtime</scope>
        </dependency>
    </dependencies>

    <properties>
        <java.version>1.8</java.version>
    </properties>

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

Thư mục code gồm có:

!image

### **Implement**

### **Tạo User**

Tạo ra class `User` tham chiếu với database.

```
import javax.persistence.Column;
import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.Id;
import javax.persistence.Table;

import lombok.Data;

@Entity
@Table(name = "user")
@Data // lombok
public class User {
    @Id
    @GeneratedValue
    private Long id;

    @Column(nullable = false, unique = true)
    private String username;
    private String password;
}
```

Tạo `UserRepository` kế thừa `JpaRepository` để truy xuất thông tin từ database.

```
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;

@Repository
public interface UserRepository extends JpaRepository<User, Long> {
    User findByUsername(String username);
}
```

### **Tham chiếu User với UserDetails**

Mặc định **Spring Security** sử dụng một đối tượng `UserDetails` để chứa toàn bộ thông tin về người dùng. Vì vậy, chúng ta cần tạo ra một class mới giúp chuyển các thông tin của `User` thành `UserDetails`

_CustomUserDetails.java_

```
import org.springframework.security.core.GrantedAuthority;
import org.springframework.security.core.authority.SimpleGrantedAuthority;
import org.springframework.security.core.userdetails.UserDetails;

import lombok.AllArgsConstructor;
import lombok.Data;

@Data
@AllArgsConstructor
public class CustomUserDetails implements UserDetails {
    User user;

    @Override
    public Collection<? extends GrantedAuthority> getAuthorities() {
        // Mặc định mình sẽ để tất cả là ROLE_USER. Để demo cho đơn giản.
        return Collections.singleton(new SimpleGrantedAuthority("ROLE_USER"));
    }

    @Override
    public String getPassword() {
        return user.getPassword();
    }

    @Override
    public String getUsername() {
        return user.getUsername();
    }

    @Override
    public boolean isAccountNonExpired() {
        return true;
    }

    @Override
    public boolean isAccountNonLocked() {
        return true;
    }

    @Override
    public boolean isCredentialsNonExpired() {
        return true;
    }

    @Override
    public boolean isEnabled() {
        return true;
    }
}
```

Khi người dùng đăng nhập thì **Spring Security** sẽ cần lấy các thông tin `UserDetails` hiện có để kiểm tra. Vì vậy, bạn cần tạo ra một class kế thừa lớp `UserDetailsService` mà **Spring Security** cung cấp để làm nhiệm vụ này.

_UserService.java_

```
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.core.userdetails.UsernameNotFoundException;
import org.springframework.stereotype.Service;

@Service
public class UserService implements UserDetailsService {

    @Autowired
    private UserRepository userRepository;

    @Override
    public UserDetails loadUserByUsername(String username) {
        // Kiểm tra xem user có tồn tại trong database không?
        User user = userRepository.findByUsername(username);
        if (user == null) {
            throw new UsernameNotFoundException(username);
        }
        return new CustomUserDetails(user);
    }

}
```

### **Cấu hình và phân quyền**

Giống với bài trước, chúng ta cần kích hoạt **Spring Security** và phân quyền người dùng.

```
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Bean;
import org.springframework.security.config.annotation.authentication.builders.AuthenticationManagerBuilder;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;
import org.springframework.security.crypto.password.PasswordEncoder;

import me.loda.spring.springsecurityhibernate.user.UserService;

@EnableWebSecurity
public class WebSecurityConfig extends WebSecurityConfigurerAdapter {

    @Autowired
    UserService userService;

    @Bean
    public PasswordEncoder passwordEncoder() {
        // Password encoder, để Spring Security sử dụng mã hóa mật khẩu người dùng
        return new BCryptPasswordEncoder();
    }

    @Override
    protected void configure(AuthenticationManagerBuilder auth)
            throws Exception {
        auth.userDetailsService(userService) // Cung cáp userservice cho spring security
            .passwordEncoder(passwordEncoder()); // cung cấp password encoder
    }

    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http
                .authorizeRequests()
                .antMatchers("/", "/home").permitAll() // Cho phép tất cả mọi người truy cập vào 2 địa chỉ này
                .anyRequest().authenticated() // Tất cả các request khác đều cần phải xác thực mới được truy cập
                .and()
                .formLogin() // Cho phép người dùng xác thực bằng form login
                .defaultSuccessUrl("/hello")
                .permitAll() // Tất cả đều được truy cập vào địa chỉ này
                .and()
                .logout() // Cho phép logout
                .permitAll();
    }
}
```

### **Tạo Controller và webpage**

Chúng ta tạo ra trang web của mình gồm có trang `homepage` và trang `hello`.

- Trang `homepage` thì ai cũng truy cập được.
- Trang `hello` thì phải xác thực mới được truy cập.

Mặc định `/login` và `/logout`**Spring Security** đã tạo cho chúng ta rồi.

```
@Controller
public class WebController {

    @GetMapping(value = {"/", "/home"})
    public String homepage() {
        return "home";
    }

    @GetMapping("/hello")
    public String hello() {
        return "hello";
    }

}
```

_hello.html_

```
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
  <title>Hello</title>
</head>
<body>
<h1>Chỉ những ai đăng nhập mới truy cập được trang này!</h1>

<form th:action="@{/logout}" method="post">
  <input type="submit" value="Sign Out"/>
</form>

</body>
</html>
```

Cú pháp `th:action="@{/logout}"` là của thymeleaf, có tác dụng request tới địa chỉ `/logout`

_home.html_

```
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
  <title>Spring Security Example</title>
</head>
<body>
<h1>Welcome!</h1>

<p><a href="/login">login</a></p>
<p>Click <a href="https://loda.me">here</a> to see a loda homepage.</p>

</body>
</html>
```

### **Tạo thông tin User trong database**

Trước hết bạn cần cấu hình cho hibernate kết tới tới h2 database trong file `resources/appication.properties`

```
spring.datasource.url=jdbc:h2:mem:testdb
spring.datasource.driverClassName=org.h2.Driver
spring.datasource.username=sa
spring.datasource.password=

spring.jpa.database-platform=org.hibernate.dialect.H2Dialect

## Enabling H2 Console
spring.h2.console.enabled=true
```

Để phục vụ demo, mỗi khi chạy chương trình, chúng ta cần insert một `User` vào database.

Có thể làm việc này bằng cách sử dụng `CommandLineRunner`. Một interface của Spring cung cấp, có tác dụng thực hiện một nhiệm vụ khi Spring khởi chạy lần đầu.

```
@SpringBootApplication
public class App implements CommandLineRunner {
    public static void main(String[] args) {
        SpringApplication.run(App.class, args);
    }

    @Autowired
    UserRepository userRepository;
    @Autowired
    PasswordEncoder passwordEncoder;

    @Override
    public void run(String... args) throws Exception {
        // Khi chương trình chạy
        // Insert vào csdl một user.
        User user = new User();
        user.setUsername("loda");
        user.setPassword(passwordEncoder.encode("loda"));
        userRepository.save(user);
        System.out.println(user);
    }
}
```

### **\#****Chạy thử**

Truy cập vào đường dẫn `http://localhost:8080/home` để vào trang chủ.

!image

Khi chưa đăng nhập, chúng ta truy cập vào đường dẫn `/hello`. Thì nó sẽ tự redirect sang trang `/login`.

!image

Khi đã đăng nhập thành công, chúng ta sẽ có thể vào trang `/hello` như bình thường.

!image

Khi click vào `Sign Out` thì sẽ đăng xuất.

!image

### **Kết**

Trong bài này, chúng ta đã tìm hiểu cách sử dụng Spring Security kết hợp với Hibernate để có thể xác thực người dùng trong cơ sở dữ liệu. Chúng ta sẽ tìm hiểu các cách xác thực `OAuth 2.0` ở các bài sau.

💁 Nếu có, toàn bộ project / code mẫu được lưu trữ tại **GitHub**

🌟 Đây là một bài viết trong Series **Làm chủ Spring Boot – Zero to Hero**

[SS] Hướng dẫn Spring Security + JWT (Json Web Token) + Hibernate
======================================================================

- Giới thiệu
- Cài đặt
- Implement
- Tạo User
- Tham chiếu User với UserDetails
- JWT
- Cấu hình và phân quyền
- Tạo Controller
- Tạo thông tin User trong database
- Chạy thử
- Kết

### **Giới thiệu**

Xin chào các bạn, Trong hai bài trước tôi đã giới thiệu cách sử dụng **Spring Security** và kết nối với database để xác thực người dùng.

1. Spring Security Cơ bản
2. Spring Security + Hibernate

Trong bài hôm nay chúng ta sẽ tìm hiểu một phần cực kỳ quan trọng trong các hệ thống bảo mật ngày nay, đó là **JWT**.

**JWT (Json web Token)** là một chuỗi mã hóa được gửi kèm trong Header của client request có tác dụng giúp phía server xác thực request người dùng có hợp lệ hay không. Được sử dụng phổ biến trong các hệ thống API ngày nay.

!image

### **Cài đặt**

Chúng ta sử dụng Maven giống bài trước, tuy nhiên có update thêm thư viện `io.jsonwebtoken.jwtt` để giúp chúng ta mã hóa thông tin thành `jwt`

_pom.xml_

```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://maven.apache.org/POM/4.0.0http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>org.springframework</groupId>
    <artifactId>spring-security-jwt</artifactId>
    <version>0.1.0</version>

    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.0.5.RELEASE</version>
    </parent>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-thymeleaf</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>com.h2database</groupId>
            <artifactId>h2</artifactId>
            <scope>runtime</scope>
        </dependency>
        <dependency>
            <groupId>io.jsonwebtoken</groupId>
            <artifactId>jjwt</artifactId>
            <version>0.9.1</version>
        </dependency>
    </dependencies>

    <properties>
        <java.version>1.8</java.version>
    </properties>

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

Cấu trúc thư mục code bao gồm:

!image

### **Implement**

Ban đầu, chúng ta sẽ tạo ra class `User` và `UserDetails` để giao tiếp với **Spring Security**. Phần này giống hệt với bài viết Spring Security + Hibernate

Trong bài viết có sử dụng Lombok

### **Tạo User**

Tạo ra class `User` tham chiếu với database.

```
import javax.persistence.Column;
import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.Id;
import javax.persistence.Table;

import lombok.Data;

@Entity
@Table(name = "user")
@Data // lombok
public class User {
    @Id
    @GeneratedValue
    private Long id;

    @Column(nullable = false, unique = true)
    private String username;
    private String password;
}
```

Tạo `UserRepository` kế thừa `JpaRepository` để truy xuất thông tin từ database.

```
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;

@Repository
public interface UserRepository extends JpaRepository<User, Long> {
    User findByUsername(String username);
}
```

### **Tham chiếu User với UserDetails**

Mặc định **Spring Security** sử dụng một đối tượng `UserDetails` để chứa toàn bộ thông tin về người dùng. Vì vậy, chúng ta cần tạo ra một class mới giúp chuyển các thông tin của `User` thành `UserDetails`

_CustomUserDetails.java_

```
import org.springframework.security.core.GrantedAuthority;
import org.springframework.security.core.authority.SimpleGrantedAuthority;
import org.springframework.security.core.userdetails.UserDetails;

import lombok.AllArgsConstructor;
import lombok.Data;

@Data
@AllArgsConstructor
public class CustomUserDetails implements UserDetails {
    User user;

    @Override
    public Collection<? extends GrantedAuthority> getAuthorities() {
        // Mặc định mình sẽ để tất cả là ROLE_USER. Để demo cho đơn giản.
        return Collections.singleton(new SimpleGrantedAuthority("ROLE_USER"));
    }

    @Override
    public String getPassword() {
        return user.getPassword();
    }

    @Override
    public String getUsername() {
        return user.getUsername();
    }

    @Override
    public boolean isAccountNonExpired() {
        return true;
    }

    @Override
    public boolean isAccountNonLocked() {
        return true;
    }

    @Override
    public boolean isCredentialsNonExpired() {
        return true;
    }

    @Override
    public boolean isEnabled() {
        return true;
    }
}
```

Khi người dùng đăng nhập thì **Spring Security** sẽ cần lấy các thông tin `UserDetails` hiện có để kiểm tra. Vì vậy, bạn cần tạo ra một class kế thừa lớp `UserDetailsService` mà **Spring Security** cung cấp để làm nhiệm vụ này.

_UserService.java_

```
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.core.userdetails.UsernameNotFoundException;
import org.springframework.stereotype.Service;

@Service
public class UserService implements UserDetailsService {

    @Autowired
    private UserRepository userRepository;

    @Override
    public UserDetails loadUserByUsername(String username) {
        // Kiểm tra xem user có tồn tại trong database không?
        User user = userRepository.findByUsername(username);
        if (user == null) {
            throw new UsernameNotFoundException(username);
        }
        return new CustomUserDetails(user);
    }

}
```

### **JWT**

Sau khi có các thông tin về người dùng, chúng ta cần mã hóa thông tin người dùng thành chuỗi `JWT`. Tôi sẽ tạo ra một class `JwtTokenProvider` để làm nhiệm vụ này.

```
import org.springframework.stereotype.Component;
import io.jsonwebtoken.*;
import lombok.extern.slf4j.Slf4j;
import me.loda.springsecurityhibernatejwt.user.CustomUserDetails;

@Component
@Slf4j
public class JwtTokenProvider {
    // Đoạn JWT_SECRET này là bí mật, chỉ có phía server biết
    private final String JWT_SECRET = "lodaaaaaa";

    //Thời gian có hiệu lực của chuỗi jwt
    private final long JWT_EXPIRATION = 604800000L;

    // Tạo ra jwt từ thông tin user
    public String generateToken(CustomUserDetails userDetails) {
        Date now = new Date();
        Date expiryDate = new Date(now.getTime() + JWT_EXPIRATION);
        // Tạo chuỗi json web token từ id của user.
        return Jwts.builder()
                   .setSubject(Long.toString(userDetails.getUser().getId()))
                   .setIssuedAt(now)
                   .setExpiration(expiryDate)
                   .signWith(SignatureAlgorithm.HS512, JWT_SECRET)
                   .compact();
    }

    // Lấy thông tin user từ jwt
    public Long getUserIdFromJWT(String token) {
        Claims claims = Jwts.parser()
                            .setSigningKey(JWT_SECRET)
                            .parseClaimsJws(token)
                            .getBody();

        return Long.parseLong(claims.getSubject());
    }

    public boolean validateToken(String authToken) {
        try {
            Jwts.parser().setSigningKey(JWT_SECRET).parseClaimsJws(authToken);
            return true;
        } catch (MalformedJwtException ex) {
            log.error("Invalid JWT token");
        } catch (ExpiredJwtException ex) {
            log.error("Expired JWT token");
        } catch (UnsupportedJwtException ex) {
            log.error("Unsupported JWT token");
        } catch (IllegalArgumentException ex) {
            log.error("JWT claims string is empty.");
        }
        return false;
    }
}
```

### **Cấu hình và phân quyền**

Bây giờ, chúng ta bắt đầu cấu hình **Spring Security** bao gồm việc kích hoạt bằng `@EnableWebSecurity`.

Bước này giống với bài viết Spring + Hibernate nên tôi sẽ không nói những phần lặp lại.

```
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Bean;
import org.springframework.security.authentication.AuthenticationManager;
import org.springframework.security.config.BeanIds;
import org.springframework.security.config.annotation.authentication.builders.AuthenticationManagerBuilder;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;
import org.springframework.security.crypto.password.PasswordEncoder;
import org.springframework.security.web.authentication.UsernamePasswordAuthenticationFilter;
import me.loda.springsecurityhibernatejwt.jwt.JwtAuthenticationFilter;
import me.loda.springsecurityhibernatejwt.user.UserService;

@EnableWebSecurity
public class WebSecurityConfig extends WebSecurityConfigurerAdapter {

    @Autowired
    UserService userService;

    @Bean
    public JwtAuthenticationFilter jwtAuthenticationFilter() {
        return new JwtAuthenticationFilter();
    }

    @Bean(BeanIds.AUTHENTICATION_MANAGER)
    @Override
    public AuthenticationManager authenticationManagerBean() throws Exception {
        // Get AuthenticationManager bean
        return super.authenticationManagerBean();
    }

    @Bean
    public PasswordEncoder passwordEncoder() {
        // Password encoder, để Spring Security sử dụng mã hóa mật khẩu người dùng
        return new BCryptPasswordEncoder();
    }

    @Override
    protected void configure(AuthenticationManagerBuilder auth)
            throws Exception {
        auth.userDetailsService(userService) // Cung cáp userservice cho spring security
            .passwordEncoder(passwordEncoder()); // cung cấp password encoder
    }

    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http
                .cors() // Ngăn chặn request từ một domain khác
                    .and()
                .authorizeRequests()
                    .antMatchers("/api/login").permitAll() // Cho phép tất cả mọi người truy cập vào địa chỉ này
                    .anyRequest().authenticated(); // Tất cả các request khác đều cần phải xác thực mới được truy cập

        // Thêm một lớp Filter kiểm tra jwt
        http.addFilterBefore(jwtAuthenticationFilter(), UsernamePasswordAuthenticationFilter.class);
    }
}
```

Điểm khác biệt ở đây là sự xuất hiện của `JwtAuthenticationFilter`. Đây là một lớp `Filter` do tôi tự tạo ra.

`JwtAuthenticationFilter` Có nhiệm vụ kiểm tra request của người dùng trước khi nó tới đích. Nó sẽ lấy `Header Authorization` ra và kiểm tra xem chuỗi JWT người dùng gửi lên có hợp lệ không.

```
@Slf4j
public class JwtAuthenticationFilter extends OncePerRequestFilter {
    @Autowired
    private JwtTokenProvider tokenProvider;

    @Autowired
    private UserService customUserDetailsService;
    @Override
    protected void doFilterInternal(HttpServletRequest request,
                                    HttpServletResponse response, FilterChain filterChain)
            throws ServletException, IOException {
        try {
            // Lấy jwt từ request
            String jwt = getJwtFromRequest(request);

            if (StringUtils.hasText(jwt) && tokenProvider.validateToken(jwt)) {
                // Lấy id user từ chuỗi jwt
                Long userId = tokenProvider.getUserIdFromJWT(jwt);
                // Lấy thông tin người dùng từ id
                UserDetails userDetails = customUserDetailsService.loadUserById(userId);
                if(userDetails != null) {
                    // Nếu người dùng hợp lệ, set thông tin cho Seturity Context
                    UsernamePasswordAuthenticationToken
                            authentication = new UsernamePasswordAuthenticationToken(userDetails, null, userDetails.getAuthorities());
                    authentication.setDetails(new WebAuthenticationDetailsSource().buildDetails(request));

                    SecurityContextHolder.getContext().setAuthentication(authentication);
                }
            }
        } catch (Exception ex) {
            log.error("failed on set user authentication", ex);
        }

        filterChain.doFilter(request, response);
    }

    private String getJwtFromRequest(HttpServletRequest request) {
        String bearerToken = request.getHeader("Authorization");
        // Kiểm tra xem header Authorization có chứa thông tin jwt không
        if (StringUtils.hasText(bearerToken) && bearerToken.startsWith("Bearer ")) {
            return bearerToken.substring(7);
        }
        return null;
    }
}
```

### **Tạo Controller**

Vì phần này chúng ta làm việc với `JWT`, nên các request sẽ dưới dạng Rest API.

Tôi tạo ra 2 api:

1. `/api/login`: Cho phép request mà không cần xác thực.
2. `/api/random`: Là một api bất kỳ nào đó, phải xác thực mới lấy được thông tin.

```
@RestController
@RequestMapping("/api")
public class LodaRestController {

    @Autowired
    AuthenticationManager authenticationManager;

    @Autowired
    private JwtTokenProvider tokenProvider;

    @PostMapping("/login")
    public LoginResponse authenticateUser(@Valid @RequestBody LoginRequest loginRequest) {

        // Xác thực từ username và password.
        Authentication authentication = authenticationManager.authenticate(
                new UsernamePasswordAuthenticationToken(
                        loginRequest.getUsername(),
                        loginRequest.getPassword()
                )
        );

        // Nếu không xảy ra exception tức là thông tin hợp lệ
        // Set thông tin authentication vào Security Context
        SecurityContextHolder.getContext().setAuthentication(authentication);

        // Trả về jwt cho người dùng.
        String jwt = tokenProvider.generateToken((CustomUserDetails) authentication.getPrincipal());
        return new LoginResponse(jwt);
    }

    // Api /api/random yêu cầu phải xác thực mới có thể request
    @GetMapping("/random")
    public RandomStuff randomStuff(){
        return new RandomStuff("JWT Hợp lệ mới có thể thấy được message này");
    }

}
```

### **Tạo thông tin User trong database**

Trước hết bạn cần cấu hình cho hibernate kết tới tới h2 database trong file `resources/appication.properties`

```
spring.datasource.url=jdbc:h2:mem:testdb
spring.datasource.driverClassName=org.h2.Driver
spring.datasource.username=sa
spring.datasource.password=

spring.jpa.database-platform=org.hibernate.dialect.H2Dialect

## Enabling H2 Console
spring.h2.console.enabled=true
```

Để phục vụ demo, mỗi khi chạy chương trình, chúng ta cần insert một `User` vào database.

Có thể làm việc này bằng cách sử dụng `CommandLineRunner`. Một interface của Spring cung cấp, có tác dụng thực hiện một nhiệm vụ khi Spring khởi chạy lần đầu.

```
@SpringBootApplication
public class App implements CommandLineRunner {
    public static void main(String[] args) {
        SpringApplication.run(App.class, args);
    }

    @Autowired
    UserRepository userRepository;
    @Autowired
    PasswordEncoder passwordEncoder;

    @Override
    public void run(String... args) throws Exception {
        // Khi chương trình chạy
        // Insert vào csdl một user.
        User user = new User();
        user.setUsername("loda");
        user.setPassword(passwordEncoder.encode("loda"));
        userRepository.save(user);
        System.out.println(user);
    }
}
```

### **Chạy thử**

Khi server on, chúng ta request thử tới địa chỉ `http://localhost:8080/api/random` mà không xác thực.

!image

Kết quả trả về mã lỗi `403` kèm theo message `Access Denied`.

Để có thể request được, chúng ta đăng nhập vào hệ thống bằng `api/login` để lấy `jwt`.

!image

Sau đó sử dụng thông tin `jwt` server trả về để thực hiện các request khác.

!image

### **Kết**

Trong bài này, chúng ta đã tìm hiểu cách sử dụng **Spring Security** và **JWT** để có thể xác thực người dùng trong các hệ thống Restful API. Chúng ta sẽ tìm hiểu các cách xác thực `OAuth 2.0` ở các bài sau.

💁 Nếu có, toàn bộ project / code mẫu được lưu trữ tại **GitHub**

🌟 Đây là một bài viết trong Series **Làm chủ Spring Boot – Zero to Hero**

