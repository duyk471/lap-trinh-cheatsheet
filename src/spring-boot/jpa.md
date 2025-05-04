「Jpa」Hibernate là gì?
===========================

- Giới thiệu
- Định nghĩa
- POJO
- Mapping dữ liệu
- Session
- Hibernate Query Language (HQL)

### **Giới thiệu**

`Hibernate` là framework được sử dụng nhiều nhất hiện nay để giúp lập trình viên Java có thể map các class (Pojo) với một cơ sở dữ liệu bất kỳ.

Trước khi `Hibernate` ra đời, chúng ta thường thao tác với cơ sở dữ liệu thông qua `JDBC`. Theo thời gian, `JDBC` bộc lộ nhiều điểm yếu như:

- Có nhiều code thừa mà chỉ phục vụ mục đích là lấy dữ liệu.
- Mất nhiều thời gian map dữ liệu vào object Java.
- Sẽ tốn nhiều công sức khi hệ thống thay đổi CSDL (yêu cầu `jdbc` mới, code mới)
- Giao tiếp giữa các bảng thường khó, thiếu tính OOP trong đó.

Từ đây, để giảm tải gánh nặng cho dev khi thao tác với database. `Hibernate` ra đời!

### **Định nghĩa**

`Hibernate` là một thư viện `ORM (Object Relational Mapping)` mã nguồn mở giúp lập trình viên viết ứng dụng Java có thể map các objects (pojo) với hệ quản trị cơ sở dữ liệu quan hệ, và hỗ trợ thực hiện các khái niệm lập trình hướng đối tượng với cớ dữ liệu quan hệ.

Hiểu ngắn gọn thì `Hibernate` sẽ là một layer đứng trung gian giữa ứng dụng và database, và chúng ta sẽ giao tiếp với `Hibernate` thay vì giao tiếp với database

!image

Để giao tiếp với `Hibernate`, chúng ta sẽ tạo ra một `Class` đại diện cho một `Table`. Và mọi dữ liệu từ `Table` trong database sẽ được `Hibernate` bind vào `Class` đó cho chúng ta.

### **POJO**

`Pojo (plain old Java object)` là class đại diện cho một `Table`, thuật ngữ này để định nghĩa chính xác thì mình không dám chắc, nhưng về ý nghĩa thì nó là một class java thuần túy, rất thuần túy:

1. All properties must public setter and getter methods (mọi biến đều phải có get/set)
2. All instance variables should be private (mọi biến là thuộc tính thì nên là private)

```
public class MyFirstPojo
{
    private String name;

    public static void main(String [] args)
    {
       for (String arg : args)
       {
          MyFirstPojo pojo = new MyFirstPojo(arg);  // Here's how you create a POJO
          System.out.println(pojo);
       }
    }

    public MyFirstPojo(String name)
    {
        this.name = name;
    }

    public String getName() { return this.name; }

    public String toString() { return this.name; }
}
```

### **Mapping dữ liệu**

Khi đã có `Class` đại diện cho `Table` rồi, chúng ta sẽ định nghĩa các trường trong class đó tương ứng với column nào trong database bằng tập hợp các `Annotaion` mà `Hibernate` cung cấp.

```
@Entity // Đánh dấu đây là một Entity, chịu sự quản lý của Hibernate
@Table(name = "USER") //Entity này đại diện cho table USER trong db
public class UserModel {
    @Id // Đánh dấu biến ở dưới là primary key của table này
    @GeneratedValue(strategy = GenerationType.IDENTITY) // Tự động tăng giá trị khi insert
    private Long id;

    @Column(name = "email", unique = true) // trường email ở dưới đại diện cho cột email trong database
    private String email;

    @Column(name = "name")
    private String name;

    public Long getId() {
        return this.id;
    }
    public void setId(Long id) {
        this.id = id;
    }
    public String getEmail() {
        return email;
    }
    public void setEmail(String email) {
        this.email = email;
    }
    public String getName() {
        return this.name;
    }
    public void setName(String name) {
        this.name = name;
    }
}
```

Bây giờ việc bạn lấy dữ liệu từ database sẽ đại loại như này:

```
public List<User> findAll() {
    return session.createQuery("SELECT a FROM User a", User.class).getResultList();
}
```

Chúng ta tiết kiệm được rất nhiều thời gian cho việc mapping dữ liệu từ database sang class java, và đặc biệt là khi thay đổi Database thì cũng sẽ không ảnh hưởng gì tới đoạn code ở trên cả, chúng ta gần như trong suốt với tầng database, mà chỉ cần nói chuyện với `Hibernate` là đủ!

### **Session**

Đối tượng chính của việc truy xuất hay insert dữ liệu bằng `Hibernate` chính là `Session` và được tạo ra từ `Session Factory`.

`Session Factory` Là một interface giúp tạo ra session kết nối đến database bằng cách đọc các cấu hình trong một file xml và mỗi loại Database khác nhau sẽ có một cấu hình khác nhau.

File cấu hình `hibernate.cfg.xml` có dạng như sau:

```
<?xml version='1.0' encoding='utf-8'?>
<!DOCTYPE hibernate-configuration PUBLIC
"-//Hibernate/Hibernate Configuration DTD//EN"
"http://www.hibernate.org/dtd/hibernate-configuration-3.0.dtd">

<hibernate-configuration><session-factory><propertyname="hibernate.connection.driver_class">com.mysql.jdbc.Driver</property><propertyname="hibernate.connection.url">jdbc:mysql://192.168.10.13:3306/loda
</property><propertyname="hibernate.connection.username">root</property><propertyname="hibernate.connection.password">root</property><propertyname="hibernate.connection.pool_size">10</property><propertyname="dialect">org.hibernate.dialect.MySQLDialect</property><propertyname="hibernate.current_session_context_class">thread</property></session-factory></hibernate-configuration>
```

Khi đã có file config này rồi, chúng ta sử dụng nó để tạo ra `Session Factory` như sau:

```
public class SessionFactoryProvider {

    private static SessionFactory buildSessionFactory() {
        ServiceRegistry serviceRegistry = new StandardServiceRegistryBuilder()//
                .configure("hibernate.cfg.xml").build();

        Metadata metadata = new MetadataSources(serviceRegistry).getMetadataBuilder().build();

        return metadata.getSessionFactoryBuilder().build();
    }
}
```

Từ đó, mỗi lần cần query hay insert dữ liệu, chúng ta sẽ tạo ra `Session` và sử dụng.

```
SessionFactory factory = HibernateSessionUtils.getSessionFactory();

Session session = factory.getCurrentSession();

try {
    session.getTransaction().begin();

    List<User> users = session.createQuery("SELECT a FROM User a", User.class).getResultList();

    session.getTransaction().commit();
}catch (Exception e) {}
```

### **Hibernate Query Language (HQL)**

`Hibernate` sử dụng ngôn ngữ `Hibernate Query Language (HQL)` để query dữ liệu. Nó chỉ khác `SQL` bình thường ở chỗ, đối tượng tác động lúc này là `Entity` chứ không còn là `Table` nữa:

ví dụ:

```
-- SQL
-- from table name
Select u.id, u.email from USER u;

-- HQL
-- from class name
Select u.id, u.email from User u;

-- query toàn bộ object
Select u from User u;
```

Đang viết dở Continue...

1. [🪂「Jpa」Hướng dẫn sử dụng @OneToOne]()
2. [🚅「Jpa」Hướng dẫn @OneToMany và @ManyToOne]()
3. [🛵「Jpa」Hướng dẫn @ManyToMany]()

💁 Nếu có, toàn bộ project / code mẫu được lưu trữ tại **GitHub**

🌟 Đây là một bài viết trong Series **Làm chủ Spring Boot – Zero to Hero**

# 「Jpa」 Hướng dẫn sử dụng Specification (Phần 1)


### **Giới thiệu**

Trong một bài viết gần nhất về  tôi đã hướng dẫn các bạn cách xây dựng các câu query xuống Database bằng các API do Hibernate cung cấp.

Và trong một bài viết khác về JPA Repository thì chúng ta cũng đã biết cách custom các query bằng cách đặt tên method:

1. 「Spring Boot #11」 Hướng dẫn Spring Boot JPA + MySQL
2. 「Spring Boot #12」 Spring JPA Method + @Query 

Tuy nhiên, trong các phương pháp trên, vẫn sẽ còn một số các điểm bất cập, ví dụ như `JpaRepository` thì bạn sẽ phải viết quá nhiều method và mỗi cái sẽ phục vụ cho một mục đích cố định (không thể tái sử dụng, reuseable).

```
public interface UserRepository extends JpaRepository<User, Long> {

  User findByEmailAddress(String emailAddress);

  List<User> findByLastname(String lastname, Sort sort);

  Page<User> findByFirstname(String firstname, Pageable pageable);
}
```

Để có thể tối ưu việc viết query một cách linh động hơn và có thể tái sử dụng lại, Spring mang tới cho chúng ta interface `Specification`.

Lúc này, cách tiếp cận của việc xây dựng query sẽ đại loại như sau:

```
userRepository.findAll(Specification.where(hasIdIn(Arrays.asList(1L, 2L, 3L, 4L, 5L)))
                                                .and(hasType(UserType.NORMAL))
                                                .or(hasId(10L)));
```

Với cách định nghĩa này, chúng ta có thể tái sử dụng query và tuỳ biến nó mọi lúc để phù hợp với yêu cầu.

Khái niệm `Specification` được xây dựng tương đương với `Predicate` trong Hibernate. Bạn hãy đọc bài dưới trước khi đi tiếp vào bài này: **Hướng dẫn sử dụng Criteria API trong Hibernate (Phần 2)** 

Trong bài có sử dụng: Lombok

### **Cài đặt**

Nhớ thêm `spring-boot-starter-data-jpa` vào dependencies của bạn.

Trong phần này, tôi xài _H2 database_ để demo. (H2 là dạng memory database, nó sẽ lưu trong ram và khi tắt chương trình nó sẽ mất sạch)

```
<?xml version="1.0" encoding="UTF-8"?>
<projectxmlns="http://maven.apache.org/POM/4.0.0"xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"xsi:schemaLocation="http://maven.apache.org/POM/4.0.0https://maven.apache.org/xsd/maven-4.0.0.xsd"><modelVersion>4.0.0</modelVersion><parent><groupId>org.springframework.boot</groupId><artifactId>spring-boot-starter-parent</artifactId><version>2.0.5.RELEASE</version><relativePath/> <!-- lookup parent from repository -->
	</parent><groupId>me.loda.spring</groupId><artifactId>example-independent-maven-spring-project</artifactId><version>0.0.1-SNAPSHOT</version><name>example-independent-maven-spring-project</name><description>Demo project for Spring Boot</description><properties><java.version>1.8</java.version></properties><dependencies><dependency><groupId>org.springframework.boot</groupId><artifactId>spring-boot-starter-web</artifactId></dependency><dependency><groupId>org.springframework.boot</groupId><artifactId>spring-boot-devtools</artifactId><scope>runtime</scope><optional>true</optional></dependency><dependency><groupId>org.projectlombok</groupId><artifactId>lombok</artifactId><optional>true</optional></dependency><dependency><groupId>org.springframework.boot</groupId><artifactId>spring-boot-starter-test</artifactId><scope>test</scope><!--			<exclusions>-->
			<!--				<exclusion>-->
			<!--					<groupId>org.junit.vintage</groupId>-->
			<!--					<artifactId>junit-vintage-engine</artifactId>-->
			<!--				</exclusion>-->
			<!--			</exclusions>-->
		</dependency><!--spring jpa-->
		<dependency><groupId>org.springframework.boot</groupId><artifactId>spring-boot-starter-data-jpa</artifactId></dependency><!--in memory database-->
		<dependency><groupId>com.h2database</groupId><artifactId>h2</artifactId><scope>runtime</scope></dependency><!--https://mvnrepository.com/artifact/org.hibernate/hibernate-jpamodelgen -->
		<dependency><groupId>org.hibernate</groupId><artifactId>hibernate-jpamodelgen</artifactId><version>5.4.9.Final</version><scope>provided</scope></dependency></dependencies><build><plugins><plugin><groupId>org.springframework.boot</groupId><artifactId>spring-boot-maven-plugin</artifactId></plugin></plugins></build></project>
```

Chú ý, xem kĩ hơn `hibernate-jpamodelgen` trong bài viết 

### **Specification**

`Specification` là một cách để định nghĩa các `Predicate` có thể tái sử dụng được.

Bản chất `Specification` là một funtional interface với 1 hàm duy nhất như sau:

```
public interface Specification<T> {
  Predicate toPredicate(Root<T> root, CriteriaQuery query, CriteriaBuilder cb);
}
```

Tham số đầu vào là 3 khái niệm tôi đã giới thiệu ở bài , bao gồm:

1. `Root`
2. `CriteriaQuery`
3. `CriteriaBuilder`

tôi sẽ demo trước một số implementation của `Specification` và đề cập cách sử dụng nó ở phía dưới:

_User.java_

```
@Entity
@Data
@Builder
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private UserType type;
    private String name;

    public enum UserType {
        NORMAL, VIP;
    }
}
```

_UserSpecification.java_

```
public final class UserSpecification {
    /**
     * Lấy ra user có UserType chỉ định
     * @param type
     * @return
     */
    public static Specification<User> hasType(UserType type) {
        return (root, query, cb) -> cb.equal(root.get(User_.TYPE), type);
    }

    /**
     * Lấy ra user có id chỉ định
     * @param userId
     * @return
     */
    public static Specification<User> hasId(long userId) {
        return (root, query, cb) -> cb.equal(root.get(User_.ID), userId);
    }

    /**
     * Lấy ra user nằm trong tập ID chỉ định
     * @param userIds
     * @return
     */
    public static Specification<User> hasIdIn(Collection<Long> userIds) {
        return (root, query, cb) -> root.get(User_.ID).in(userIds);
    }
}
```

Tôi định nghĩa ra các `static method` trả ra ngoài là các implement của `Specification`. Nó không hẳn là đoạn code đẹp nhất ==! nhưng nó là một cách làm hay để có thể định nghĩa một tập các điều kiện có thể sử dụng lại được cho `User`.

### **JpaSpecificationExecutor**

Để có thể sử dụng được `Specification`, bạn cần kế thừa `JpaSpecificationExecutor` từ Spring JPA

```
public interface UserRepository extends JpaRepository<User, Long>,
                                        JpaSpecificationExecutor<User> {
}
```

Lúc này, ngoài các method truyền thống như `findAll()`, `findOne()`, `findBy()` thì bạn sẽ thấy xuất hiện các method mới có tham số đầu vào là `Specification<T>`:

```
Optional<T> findOne(@Nullable Specification<T> var1);

List<T> findAll(@Nullable Specification<T> var1);

Page<T> findAll(@Nullable Specification<T> var1, Pageable var2);

List<T> findAll(@Nullable Specification<T> var1, Sort var2);

long count(@Nullable Specification<T> var1);
```

### **Usage**

Lúc này, để sử dụng, bạn gọi `Specification.where()` để xây dựng cho mình tập các điều kiện để query

```
// Lấy ra user nằm trong tập ID đã cho và có type là NORMAL
// hoặc lấy ra user có ID = 10
Specification conditions = Specification.where(UserSpecification.hasIdIn(Arrays.asList(1L, 2L, 3L, 4L, 5L)))
                                        .and(UserSpecification.hasType(UserType.NORMAL))
                                        .or(UserSpecification.hasId(10L));
// Truyền Specification vào hàm findAll()
userRepository.findAll(conditions).forEach(System.out::println);
```

OUTPUT:

```
User(id=1, type=NORMAL, name=name-0)
User(id=2, type=NORMAL, name=name-1)
User(id=5, type=NORMAL, name=name-4)
User(id=10, type=VIP, name=name-9)
```

Ngoài ra, bạn có thể import trực tiếp các hàm static vào để code gọn hơn:

```
import static me.loda.spring.specification.User.UserType.NORMAL;
import static me.loda.spring.specification.UserSpecification.*;

...
// Lấy ra user nằm trong tập ID đã cho và có type là NORMAL
// hoặc lấy ra user có ID = 10
Specification conditions = Specification.where(hasIdIn(Arrays.asList(1L, 2L, 3L, 4L, 5L)))
                                        .and(hasType(NORMAL))
                                        .or(hasId(10L));
// Truyền Specification vào hàm findAll()
userRepository.findAll(conditions).forEach(System.out::println);
```

Gọn hơn thì không nên tạo ra 1 biến tham chiếu thừa thãi:

```
userRepository.findAll(Specification.where(hasIdIn(Arrays.asList(1L, 2L, 3L, 4L, 5L)))
                                    .and(hasType(NORMAL))
                                    .or(hasId(10L)))
              .forEach(System.out::println);
```

### **Kết**

Tới đây bạn đã có thể sử dụng `Specification` để thực hiện các truy vấn phức tạp và tái sử dụng được trong nhiều trường hợp. Đón đọc các bài sau về các phần nâng cao hơn.





「Jpa」Hướng dẫn sử dụng @OneToOne
=============================================

- Giới thiệu
- Tạo project
- Tạo Table
- Chạy thử
- Thêm dữ liệu

### **Giới thiệu**

Cách biểu thị quan hệ 1-1 trong cơ sở dữ liệu là rất phổ biến, ví dụ một người sẽ có một địa chỉ duy nhất (giả sử).

Bình thường, khi các bạn tạo table trong csdl để biểu thị mối quan hệ này, thì sẽ có một bảng chứa khóa ngoại của bảng còn lại.

!image

Thể hiện mỗi quan hệ này trong `code` bằng `Hibernate` thì chúng ta sẽ dùng `@OneToOne`.

Trong bài sử dụng các kiến thức:

1. Hibernate là gì?
2. Cách sử dụng Lombok để tiết kiệm thời gian code

### **Tạo project**

Toàn bộ bài viết được up tại `Github`: github.com/loda-kun/java-all

Chúng ta sẽ sử dụng `Gradle` để tạo một project có khai báo `Spring Boot` và `Jpa` để hỗ trợ cho việc demo `@OneToOne`.

Các bạn có thể tự tạo 1 project Spring-boot với gradle đơn giản tại: https://start.spring.io

```
plugins {
    id 'org.springframework.boot' version '2.1.4.RELEASE'
    id 'java'
}
apply plugin: 'io.spring.dependency-management'

group 'me.loda.java'
version '1.0-SNAPSHOT'

sourceCompatibility = 1.8

configurations {
    compileOnly {
        extendsFrom annotationProcessor
    }
}

repositories {
    mavenCentral()
}

dependencies {
    implementation 'org.springframework.boot:spring-boot-starter-data-jpa'
    implementation 'org.springframework.boot:spring-boot-starter-web'
    compileOnly 'org.projectlombok:lombok'
    runtimeOnly 'com.h2database:h2'
    annotationProcessor 'org.projectlombok:lombok'
    testImplementation 'org.springframework.boot:spring-boot-starter-test'
}
```

Trong ứng dụng trên bạn sẽ thấy có `com.h2database:h2`. Đây là một **database**, tuy nhiên nó chỉ tồn tại trong bộ nhớ. Tức làm mỗi khi chạy chương trình này, nó sẽ tạo database trong `RAM`, và tắt chương trình đi nó sẽ mất.

Chúng ta sẽ sử dụng `H2` thay cho `MySql` để cho.. tiện!

Khi tạo xong project, sẽ có thư mục như sau:

!image

### **Tạo Table**

Để tạo table, chúng ta tạo ra các `Class` tương ứng.

```
import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.Id;
import javax.persistence.JoinColumn;
import javax.persistence.OneToOne;

import lombok.Builder;
import lombok.Data;

@Entity // Hibernate entity
@Data // Lombok
@Builder // Lombok
public class Person { //Table person

    @Id // Đánh dấu trường này là primary key
    @GeneratedValue // Tự động tăng giá trị id
    private Long id;
    private String name;
}
```

```
@Entity
@Data
@Builder
public class Address { // Table address
    @Id
    @GeneratedValue
    private Long id;

    private String city;
    private String province;

    @OneToOne // Đánh dấu có mỗi quan hệ 1-1 với Person ở phía dưới
    @JoinColumn(name = "person_id") // Liên kết với nhau qua khóa ngoại person_id
    private Person person;
}
```

Nếu chúng ta chưa tạo ra các table trong cơ sở dữ liệu, thì mặc định `Hibernate` sẽ bind dữ liệu từ class xuống và tạo table cho chúng ta.

Bạn phải tạo file config `src\main\resources\application.properties` như sau để kết nối tới `H2` database nhé:

```
spring.datasource.url=jdbc:h2:mem:testdb
spring.datasource.driverClassName=org.h2.Driver
spring.datasource.username=sa
spring.datasource.password=
// Không có password, vào thẳng luôn
spring.jpa.database-platform=org.hibernate.dialect.H2Dialect
# Cho phép vào xem db thông qua web
spring.h2.console.enabled=true
```

### **Chạy thử**

Bạn tạo file `OneToOneExampleApplication` và cấu hình `Spring Boot` và khởi chạy chương trình.

```
@SpringBootApplication
@RequiredArgsConstructor
public class OneToOneExampleApplication {
    public static void main(String[] args) {
        SpringApplication.run(OneToOneExampleApplication.class, args);
    }
}
```

Sau khi chạy xong, hãy truy cập vào `http://localhost:8080/h2-console/` để vào xem database có gì nhé.

!image

Bạn sẽ thấy nó tạo table giống với mô tả ở đầu bài. Với khóa ngoại `person_id` ở bảng `address`.

### **Thêm dữ liệu**

Để thêm dữ liệu vào database, chúng ta sẽ dùng tới `Jpa`.

```
import org.springframework.data.jpa.repository.JpaRepository;

public interface AddressRepository extends JpaRepository<Address,Long> {
}
public interface PersonRepository extends JpaRepository<Person, Long> {
}
```

Chúng ta sẽ tạo một chương trình `Spring Boot` đơn giản bằng cách sử dụng `CommandLineRunner` để chạy code ngay khi khởi động.

```
import javax.transaction.Transactional;

import org.springframework.boot.CommandLineRunner;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

import com.google.common.collect.Lists;

import lombok.RequiredArgsConstructor;

@SpringBootApplication
@RequiredArgsConstructor
public class OneToOneExampleApplication implements CommandLineRunner {
    public static void main(String[] args) {
        SpringApplication.run(OneToOneExampleApplication.class, args);
    }

    // Sử dụng @RequiredArgsConstructor và final để thay cho @Autowired
    private final PersonRepository personRepository;
    private final AddressRepository addressRepository;

    @Override
    public void run(String... args) throws Exception {
        // Tạo ra đối tượng person
        Person person = Person.builder()
                              .name("loda")
                              .build();
        // Lưu vào db
        personRepository.save(person);

        // Tạo ra đối tượng Address có tham chiếu tới person
        Address address = Address.builder()
                .city("Hanoi")
                .person(person)
                .build();

        // Lưu vào db
        addressRepository.save(address);

        // Vào:http://localhost:8080/h2-console/ để xem dữ liệu đã insert
    }
}
```

Kết quả trong database lúc này:

!image

Vậy là thằng `Address` đã liên kết tới `Person` có `id=1`. Đúng như ta mong đợi.

Bài viết của mình không còn gì để ngắn hơn được nữa :((( thật hổ thẹn, mình có up code lên đây, bạn chạy code cái là hiểu liền à:

Chúc các bạn học tập tốt! ahuu

1. [🚅「Jpa」Hướng dẫn @OneToMany và @ManyToOne]()
2. [🛵「Jpa」Hướng dẫn @ManyToMany]()

💁 Nếu có, toàn bộ project / code mẫu được lưu trữ tại **GitHub**

🌟 Đây là một bài viết trong Series **Làm chủ Spring Boot – Zero to Hero**

「Jpa」Hướng dẫn @OneToMany và @ManyToOne
=================================================

- Giới thiệu
- Tạo project
- Tạo Table
- Chạy thử
- Thêm dữ liệu

### **Giới thiệu**

Cách biểu thị quan hệ 1-n trong cơ sở dữ liệu là rất phổ biến, ví dụ một địa chỉ có thể có nhiều người ở (gia đình).

Bình thường, khi các bạn tạo table trong csdl để biểu thị mối quan hệ này, thì bảng đại diện phía nhiều (phía n trong câu 1-n) sẽ chứa id của bảng tham chiếu (phía 1 trong câu 1-n)

!image

Thể hiện mỗi quan hệ này một cách đầy đủ trong `code` bằng `Hibernate` thì chúng ta sẽ dùng `@OneToMany` và `@ManyToOne`.

Trong bài sử dụng các kiến thức:

1. Hibernate là gì?
2. Cách sử dụng Lombok để tiết kiệm thời gian code

### **Tạo project**

Toàn bộ bài viết được up tại `Github`: github.com/loda-kun/java-all

Chúng ta sẽ sử dụng `Gradle` để tạo một project có khai báo `Spring Boot` và `Jpa` để hỗ trợ cho việc demo `@ManyToOne`.

Các bạn có thể tự tạo 1 project Spring-boot với gradle đơn giản tại: https://start.spring.io

```
plugins {
    id 'org.springframework.boot' version '2.1.4.RELEASE'
    id 'java'
}
apply plugin: 'io.spring.dependency-management'

group 'me.loda.java'
version '1.0-SNAPSHOT'

sourceCompatibility = 1.8

configurations {
    compileOnly {
        extendsFrom annotationProcessor
    }
}

repositories {
    mavenCentral()
}

dependencies {
    implementation 'org.springframework.boot:spring-boot-starter-data-jpa'
    implementation 'org.springframework.boot:spring-boot-starter-web'
    compileOnly 'org.projectlombok:lombok'
    runtimeOnly 'com.h2database:h2'
    annotationProcessor 'org.projectlombok:lombok'
    testImplementation 'org.springframework.boot:spring-boot-starter-test'
}
```

Trong ứng dụng trên bạn sẽ thấy có `com.h2database:h2`. Đây là một **database**, tuy nhiên nó chỉ tồn tại trong bộ nhớ. Tức làm mỗi khi chạy chương trình này, nó sẽ tạo database trong `RAM`, và tắt chương trình đi nó sẽ mất.

Chúng ta sẽ sử dụng `H2` thay cho `MySql` để cho.. tiện!

Khi tạo xong project, sẽ có thư mục như sau:

!image

### **Tạo Table**

Để tạo table, chúng ta tạo ra các `Class` tương ứng.

```
import java.util.Collection;

import javax.persistence.CascadeType;
import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.Id;
import javax.persistence.OneToMany;

import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.EqualsAndHashCode;
import lombok.NoArgsConstructor;
import lombok.ToString;

@Entity // Đánh dấu đây là table trong db
@Data // lombok giúp generate các hàm constructor, get, set v.v.
@AllArgsConstructor
@NoArgsConstructor
public class Address {

    @Id //Đánh dấu là primary key
    @GeneratedValue // Giúp tự động tăng
    private Long id;

    private String city;
    private String province;

    @OneToMany(mappedBy = "address", cascade = CascadeType.ALL) // Quan hệ 1-n với đối tượng ở dưới (Person) (1 địa điểm có nhiều người ở)
    // MapopedBy trỏ tới tên biến Address ở trong Person.
    @EqualsAndHashCode.Exclude // không sử dụng trường này trong equals và hashcode
    @ToString.Exclude // Khoonhg sử dụng trong toString()
    private Collection<Person> persons;
}
```

```
@Entity
@Data
@NoArgsConstructor
@AllArgsConstructor
public class Person {

    @Id
    @GeneratedValue
    private Long id;
    private String name;

    // Many to One Có nhiều người ở 1 địa điểm.
    @ManyToOne
    @JoinColumn(name = "address_id") // thông qua khóa ngoại address_id
    @EqualsAndHashCode.Exclude
    @ToString.Exclude
    private Address address;
}
```

Nếu chúng ta chưa tạo ra các table trong cơ sở dữ liệu, thì mặc định `Hibernate` sẽ bind dữ liệu từ class xuống và tạo table cho chúng ta.

Bạn phải tạo file config `src\main\resources\application.properties` như sau để kết nối tới `H2` database nhé:

```
spring.datasource.url=jdbc:h2:mem:testdb
spring.datasource.driverClassName=org.h2.Driver
spring.datasource.username=sa
spring.datasource.password=
// Không có password, vào thẳng luôn
spring.jpa.database-platform=org.hibernate.dialect.H2Dialect
# Cho phép vào xem db thông qua web
spring.h2.console.enabled=true
```

### **Chạy thử**

Bạn tạo file `OneToManyExampleApplication` và cấu hình `Spring Boot` và khởi chạy chương trình.

```
@SpringBootApplication
@RequiredArgsConstructor
public class OneToManyExampleApplication {
    public static void main(String[] args) {
        SpringApplication.run(OneToManyExampleApplication.class, args);
    }
}
```

Sau khi chạy xong, hãy truy cập vào `http://localhost:8080/h2-console/` để vào xem database có gì nhé.

!image

Bạn sẽ thấy nó tạo table giống với mô tả ở đầu bài. Với khóa ngoại `address_id` ở bảng `person`.

### **Thêm dữ liệu**

Để thêm dữ liệu vào database, chúng ta sẽ dùng tới `Spring JPA` .

```
import org.springframework.data.jpa.repository.JpaRepository;

public interface AddressRepository extends JpaRepository<Address,Long> {
}
public interface PersonRepository extends JpaRepository<Person, Long> {
}
```

Chúng ta sẽ tạo một chương trình `Spring Boot` đơn giản bằng cách sử dụng `CommandLineRunner` để chạy code ngay khi khởi động.

```
import javax.transaction.Transactional;

import org.springframework.boot.CommandLineRunner;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

import com.google.common.collect.Lists;

import lombok.RequiredArgsConstructor;

@SpringBootApplication
@RequiredArgsConstructor
public class OneToManyExampleApplication implements CommandLineRunner {
    public static void main(String[] args) {
        SpringApplication.run(OneToManyExampleApplication.class, args);
    }

    // Sử dụng @RequiredArgsConstructor và final để thay cho @Autowired
    private final PersonRepository personRepository;
    private final AddressRepository addressRepository;

    @Override
    public void run(String... args) throws Exception {
        // Tạo ra đối tượng Address có tham chiếu tới person
        Address address = new Address();
        address.setCity("Hanoi");

        // Tạo ra đối tượng person
        Person person = new Person();
        person.setName("loda");
        person.setAddress(address);

        address.setPersons(Collections.singleton(person));
        // Lưu vào db
        // Chúng ta chỉ cần lưu address, vì cascade = CascadeType.ALL nên nó sẽ lưu luôn Person.
        addressRepository.saveAndFlush(address);

        // Vào:http://localhost:8080/h2-console/ để xem dữ liệu đã insert

        personRepository.findAll().forEach(p -> {
            System.out.println(p.getId());
            System.out.println(p.getName());
            System.out.println(p.getAddress());
        });

    }
}
//output:
// 2
// loda
// Address(id=1, city=Hanoi, province=null)
// Chúng ta đã có thể gọi trực tiếp address trong person sau khi query
```

Kết quả trong database lúc này:

!image

Bài viết của mình không còn gì để ngắn hơn được nữa :((( thật hổ thẹn, mình có up code lên đây, bạn chạy code cái là hiểu liền à:

Chúc các bạn học tập thật tốt! ahuu

1. [🪂「Jpa」Hướng dẫn sử dụng @OneToOne]()
2. [🛵「Jpa」Hướng dẫn @ManyToMany]()

💁 Nếu có, toàn bộ project / code mẫu được lưu trữ tại **GitHub**

🌟 Đây là một bài viết trong Series **Làm chủ Spring Boot – Zero to Hero**

「Jpa」Hướng dẫn @ManyToMany
===================================

- Giới thiệu
- Tạo project
- Tạo Table
- Chạy thử
- Thêm dữ liệu

### **Giới thiệu**

Cách biểu thị quan hệ n-n trong cơ sở dữ liệu là rất phổ biến, ví dụ một địa chỉ có thể có nhiều người ở (gia đình). và một người có thể có nhiều hơn một địa chỉ.

Bình thường, khi các bạn tạo table trong csdl để biểu thị mối quan hệ này, chúng ta sẽ tạo ra một bảng mới, tham chiếu tới cả bảng này.

!image

Thể hiện mỗi quan hệ này một cách đầy đủ trong `code` bằng `Hibernate` thì chúng ta sẽ dùng `@ManyToMany`

Trong bài sử dụng các kiến thức:

1. Hibernate là gì?
2. Cách sử dụng Lombok để tiết kiệm thời gian code

### **Tạo project**

Toàn bộ bài viết được up tại `Github`: github.com/loda-kun/java-all

Chúng ta sẽ sử dụng `Gradle` để tạo một project có khai báo `Spring Boot` và `Jpa` để hỗ trợ cho việc demo `@ManyToMany`.

Các bạn có thể tự tạo 1 project Spring-boot với gradle đơn giản tại: https://start.spring.io

```
plugins {
    id 'org.springframework.boot' version '2.1.4.RELEASE'
    id 'java'
}
apply plugin: 'io.spring.dependency-management'

group 'me.loda.java'
version '1.0-SNAPSHOT'

sourceCompatibility = 1.8

configurations {
    compileOnly {
        extendsFrom annotationProcessor
    }
}

repositories {
    mavenCentral()
}

dependencies {
    implementation 'org.springframework.boot:spring-boot-starter-data-jpa'
    implementation 'org.springframework.boot:spring-boot-starter-web'
    compileOnly 'org.projectlombok:lombok'
    runtimeOnly 'com.h2database:h2'
    annotationProcessor 'org.projectlombok:lombok'
    testImplementation 'org.springframework.boot:spring-boot-starter-test'
}
```

Trong ứng dụng trên bạn sẽ thấy có `com.h2database:h2`. Đây là một **database**, tuy nhiên nó chỉ tồn tại trong bộ nhớ. Tức làm mỗi khi chạy chương trình này, nó sẽ tạo database trong `RAM`, và tắt chương trình đi nó sẽ mất.

Chúng ta sẽ sử dụng `H2` thay cho `MySql` để cho.. tiện!

Khi tạo xong project, sẽ có thư mục như sau:

!image

### **Tạo Table**

Để tạo table, chúng ta tạo ra các `Class` tương ứng.

```
import java.util.Collection;

import javax.persistence.CascadeType;
import javax.persistence.Entity;
import javax.persistence.FetchType;
import javax.persistence.GeneratedValue;
import javax.persistence.Id;
import javax.persistence.JoinColumn;
import javax.persistence.JoinTable;
import javax.persistence.ManyToMany;

import lombok.AllArgsConstructor;
import lombok.Builder;
import lombok.Data;
import lombok.EqualsAndHashCode;
import lombok.NoArgsConstructor;
import lombok.ToString;

@Entity // Đánh dấu đây là table trong db
@Data // lombok giúp generate các hàm constructor, get, set v.v.
@AllArgsConstructor
@NoArgsConstructor
@Builder
public class Address {

    @Id //Đánh dấu là primary key
    @GeneratedValue // Giúp tự động tăng
    private Long id;

    private String city;
    private String province;

    @ManyToMany(cascade = CascadeType.ALL, fetch = FetchType.LAZY)
    // Quan hệ n-n với đối tượng ở dưới (Person) (1 địa điểm có nhiều người ở)
    @EqualsAndHashCode.Exclude // không sử dụng trường này trong equals và hashcode
    @ToString.Exclude // Khoonhg sử dụng trong toString()

    @JoinTable(name = "address_person", //Tạo ra một join Table tên là "address_person"
            joinColumns = @JoinColumn(name = "address_id"),  // TRong đó, khóa ngoại chính là address_id trỏ tới class hiện tại (Address)
            inverseJoinColumns = @JoinColumn(name = "person_id") //Khóa ngoại thứ 2 trỏ tới thuộc tính ở dưới (Person)
    )
    private Collection<Person> persons;
}
```

```
@Entity
@Data
@NoArgsConstructor
@AllArgsConstructor
@Builder
public class Person {

    @Id
    @GeneratedValue
    private Long id;
    private String name;

    // mappedBy trỏ tới tên biến persons ở trong Address.
    @ManyToMany(mappedBy = "persons")
    // LAZY để tránh việc truy xuất dữ liệu không cần thiết. Lúc nào cần thì mới query
    @EqualsAndHashCode.Exclude
    @Exclude
    private Collection<Address> addresses;
}
```

Nếu chúng ta chưa tạo ra các table trong cơ sở dữ liệu, thì mặc định `Hibernate` sẽ bind dữ liệu từ class xuống và tạo table cho chúng ta.

Bạn phải tạo file config `src\main\resources\application.properties` như sau để kết nối tới `H2` database nhé:

```
spring.datasource.url=jdbc:h2:mem:testdb
spring.datasource.driverClassName=org.h2.Driver
spring.datasource.username=sa
spring.datasource.password=
// Không có password, vào thẳng luôn
spring.jpa.database-platform=org.hibernate.dialect.H2Dialect
# Cho phép vào xem db thông qua web
spring.h2.console.enabled=true
```

### **Chạy thử**

Bạn tạo file `ManyToManyExampleApplication` và cấu hình `Spring Boot` và khởi chạy chương trình.

```
@SpringBootApplication
@RequiredArgsConstructor
public class ManyToManyExampleApplication {
    public static void main(String[] args) {
        SpringApplication.run(ManyToManyExampleApplication.class, args);
    }
}
```

Sau khi chạy xong, hãy truy cập vào `http://localhost:8080/h2-console/` để vào xem database có gì nhé.

!image

Bạn sẽ thấy nó tạo table giống với mô tả ở đầu bài. Gồm có hai bảng chính là `address` và `person`. Ngoài ra, sẽ tạo ra một bảng trung gian ở giữa liên kết hai bảng là `address_person`.

### **Thêm dữ liệu**

Để thêm dữ liệu vào database, chúng ta sẽ dùng tới `Spring JPA` .

```
import org.springframework.data.jpa.repository.JpaRepository;

public interface AddressRepository extends JpaRepository<Address,Long> {
}
public interface PersonRepository extends JpaRepository<Person, Long> {
}
```

Chúng ta sẽ tạo một chương trình `Spring Boot` đơn giản bằng cách sử dụng `CommandLineRunner` để chạy code ngay khi khởi động.

```
import javax.transaction.Transactional;

import org.springframework.boot.CommandLineRunner;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

import com.google.common.collect.Lists;

import lombok.RequiredArgsConstructor;

@SpringBootApplication
@RequiredArgsConstructor
public class ManyToManyExampleApplication implements CommandLineRunner {
    public static void main(String[] args) {
        SpringApplication.run(ManyToManyExampleApplication.class, args);
    }

    // Sử dụng @RequiredArgsConstructor và final để thay cho @Autowired
    private final PersonRepository personRepository;
    private final AddressRepository addressRepository;

    @Override
    @Transactional
    public void run(String... args) throws Exception {
        // Tạo ra đối tượng Address
        Address hanoi = Address.builder()
                                 .city("hanoi")
                                 .build();
        Address hatay = Address.builder()
                               .city("hatay")
                               .build();

        // Tạo ra đối tượng person
        Person person1 = Person.builder()
                              .name("loda1")
                              .build();
        Person person2 = Person.builder()
                              .name("loda2")
                              .build();

        // set Persons vào address
        hanoi.setPersons(Lists.newArrayList(person1, person2));
        hatay.setPersons(Lists.newArrayList(person1));

        // Lưu vào db
        // Chúng ta chỉ cần lưu address, vì cascade = CascadeType.ALL nên nó sẽ lưu luôn Person.
        addressRepository.saveAndFlush(hanoi);
        addressRepository.saveAndFlush(hatay);

        // Vào:http://localhost:8080/h2-console/ để xem dữ liệu đã insert

        Address queryResult = addressRepository.findById(1L).get();
        System.out.println(queryResult.getCity());
        System.out.println(queryResult.getPersons());

    }

}
// Output:
// hanoi
// [Person(id=2, name=loda1), Person(id=3, name=loda2)]
```

Lưu ý ở đây chúng ta dùng `@Transactional`. Đê khiến toàn bộ code chạy trong hàm đều nằm trong `Session` quản lý của `Hibernate`.

Nếu không có `@Transactional` thì việc bạn gọi `address.getPersons()` sẽ bị lỗi, vì nó không thể query xuống database để lấy dữ liệu person lên được. Bạn ghi nhớ chỗ này nhé.

Kết quả trong database lúc này:

`Address`

!image

!image

!image

Bài viết của mình không còn gì để ngắn hơn được nữa :((( thật hổ thẹn, mình có up code lên đây, bạn chạy code cái là hiểu liền à:

Chúc các bạn học tập thật tốt! ahuu

1. [🪂「Jpa」Hướng dẫn sử dụng @OneToOne]()
2. [🚅「Jpa」Hướng dẫn @OneToMany và @ManyToOne]()

💁 Nếu có, toàn bộ project / code mẫu được lưu trữ tại **GitHub**

🌟 Đây là một bài viết trong Series **Làm chủ Spring Boot – Zero to Hero**

「Jpa」Hướng dẫn Query phân trang bằng Pageable (Phần 1)
====================================================================

- Giới thiệu
- Cài đặt
- Tạo Model và Repository
- Pageable
- Sorting
- Note
- Ví dụ DEMO
- Kết

### **Giới thiệu**

Điều này có lẽ ai cũng biết, đó là đa phần chúng ta không lấy toàn bộ dữ liệu từ Database lên, mà chỉ lấy một số lượng nhất định, và chia nó thành nhiều trang.

Mà cái blog này cũng là ví dụ điển hình:

!image

Tôi chia dữ liệu thành nhiều trang khác nhau, với mỗi trang thì sẽ lấy ra các bài viết cần thiết.

Tạo sao cần dùng phân trang? vì nó giúp tiết kiệm băng thông và tăng tốc xử lý, ngoài ra nó cũng giảm thiểu việc hiển thị các thông tin thừa không cần thiết.

Chúng ta sẽ tìm hiểu các làm việc này bằng `JPA Pageable`

Nếu chưa biết Spring JPA thì xem tại đây:

1. Hibernate là gì?
2. Spring JPA

### **Cài đặt**

Nhớ thêm `spring-boot-starter-data-jpa` vào dependencies của bạn.

Trong phần này, tôi xài H2 database để demo. (h2 là dạng memory database, nó sẽ lưu trong ram và khi tắt chương trình nó sẽ mất sạch)

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

        <!--spring jpa-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-jpa</artifactId>
        </dependency>

        <!-- h2 database -->

        <dependency>
            <groupId>com.h2database</groupId>
            <artifactId>h2</artifactId>
            <scope>runtime</scope>
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

### **Tạo Model và Repository**

Tạo ra class `User` và insert sẵn 100 `User` vào Database.

_User.java_

```
@Entity
@Data
@Builder
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;
}
```

_UserRepository.java_

```
public interface UserRepository extends JpaRepository<User, Long> {

}
```

_DatasourceConfig.java_

```
@Configuration
@RequiredArgsConstructor
public class DatasourceConfig {
    // inject bởi RequiredArgsConstructor
    private final UserRepository userRepository;

    // Chỉ áp dụng trong demo :D
    @PostConstruct
    public void initData() {
        // Insert 100 User vào H2 Database sau khi
        // DatasourceConfig được khởi tạo
        userRepository.saveAll(IntStream.range(0, 100)
                                        .mapToObj(i -> User.builder()
                                                           .name("name-" + i)
                                                           .build())
                                        .collect(Collectors.toList())
        );
    }
}
```

### **Pageable**

Ukie, giả sử bây giờ chúng ta có 100 record trong DB rồi.

Để có thể query lấy dữ liệu theo dạng Page, **Spring Data JPA** hỗ trợ chúng ta bằng đối tượng `Pageable`.

Hàm `findAll(Pageable pageable)` là có sẵn và trả về đối tượng `Page<T>`

```
// Lấy ra 5 user đầu tiên
// PageRequest.of(0,5) tương đương với lấy ra page đầu tiên, và mỗi page sẽ có 5 phần tử
// PageRequest là một đối tượng kế thừa Pageable
Page<User> page = userRepository.findAll(PageRequest.of(0, 5));
```

Để lấy ra 5 `User` tiếp theo, chúng ta có thể làm theo 2 cách:

```
// tận dụng đối tượng Page trước đó
page.nextPageable()

// Sử dụng PageRequest mới
PageRequest.of(1, 5)
```

### **Sorting**

Bạn có thể query dạng `Page` kèm theo yêu cầu sorting theo một trường nào đó.

```
Page<User> page = userRepository.findAll(PageRequest.of(1, 5, Sort.by("name").descending()));
```

### **Note**

Ngoài ra, bạn hoàn toàn có thể tự custom để hàm trả về `Page<T>`, `Slice<T>`, `List<T>`.

```
public interface UserRepository extends JpaRepository<User, Long> {
    List<User> findAllByNameLike(String name, Pageable pageable);
}
```

### **Ví dụ DEMO**

```
@SpringBootApplication
@RequiredArgsConstructor
public class App {
    public static void main(String[] args) {
        SpringApplication.run(App.class, args);
    }

    private final UserRepository userRepository;

    @Bean
    CommandLineRunner run() {
        return args -> {
            // Lấy ra 5 user đầu tiên
            // PageRequest.of(0,5) tương đương với lấy ra page đầu tiên, và mỗi page sẽ có 5 phần tử
            Page<User> page = userRepository.findAll(PageRequest.of(0, 5));
            // In ra 5 user đầu tiên
            System.out.println("In ra 5 user đầu tiên: ");
            page.forEach(System.out::println);
            // Lấy ra 5 user tiếp theo
            page = userRepository.findAll(page.nextPageable());

            System.out.println("In ra 5 user tiếp theo: ");
            page.forEach(System.out::println);

            System.out.println("In ra số lượng user ở page hiện tại: " + page.getSize());
            System.out.println("In ra tổng số lượng user: " + page.getTotalElements());
            System.out.println("In ra tổng số page: " + page.getTotalPages());

            // Lấy ra 5 user ở page 1, sort theo tên
            page = userRepository.findAll(PageRequest.of(1, 5, Sort.by("name").descending()));
            System.out.println("In ra 5 user page 1, sắp xếp theo name descending:");
            page.forEach(System.out::println);

            // Custom method
            List<User> list = userRepository.findAllByNameLike("name-%", PageRequest.of(0, 5));
            System.out.println(list);
        };
    }

}
```

Output:

```
In ra 5 user đầu tiên:
User(id=1, name=name-0)
User(id=2, name=name-1)
User(id=3, name=name-2)
User(id=4, name=name-3)
User(id=5, name=name-4)

In ra 5 user tiếp theo:
User(id=6, name=name-5)
User(id=7, name=name-6)
User(id=8, name=name-7)
User(id=9, name=name-8)
User(id=10, name=name-9)

In ra số lượng user ở page hiện tại: 5
In ra tổng số lượng user: 100
In ra tổng số page: 20

In ra 5 user page 1, sắp xếp theo name descending:
User(id=95, name=name-94)
User(id=94, name=name-93)
User(id=93, name=name-92)
User(id=92, name=name-91)
User(id=91, name=name-90)

Custom Method
[User(id=1, name=name-0), User(id=2, name=name-1), User(id=3, name=name-2), User(id=4, name=name-3), User(id=5, name=name-4)]
```

### **Kết**

`Pageable` là một cách tiếp cận rất tiện lợi, tuy nhiên bạn cần biết rằng các hàm sử dụng `Pageable` sẽ thực hiện query 2 lần xuống DB. Một lần là để lấy ra tổng số lượng bản ghi, một lần là query lấy ra page bạn cần. Cái này tôi sẽ nói kĩ hơn ở bài sau để chúng ta tối ưu hiệu năng

💁 Nếu có, toàn bộ project / code mẫu được lưu trữ tại **GitHub**

🌟 Đây là một bài viết trong Series **Làm chủ Spring Boot – Zero to Hero**

# 「Jpa」Hướng dẫn sử dụng Criteria API trong Hibernate

`JPA Criteria API` cho phép ta tạo ra các câu truy vấn bằng Java Object thay vì việc khai báo trực tiếp trong `String` (JPQL) như thế này:

Tương đương với câu lệnh trên nhưng xây dựng bằng `Criteria API` thì sẽ như này:

```
CriteriaBuilder cb = em.getCriteriaBuilder();

CriteriaQuery<Office> q = cb.createQuery(Office.class);
Root<Office> c = q.from(Office.class);
q.select(c);
```

Nhìn có vẻ dài dòng và khó hiểu phải không >"< Yea, thì đúng là như thế đấy ==!

Nếu nó dài dòng như vậy, tại sao người ta lại tạo ra và sử dụng nó thay cho câu lệnh `JPQL` bình thường? Các bạn đọc phần tiếp theo sẽ rõ nha.

### **JPQL vs Criteria API**

`JPQL` có thể làm đầy đủ chức năng chúng ta cần chỉ với 1 câu lệnh, tuy nhiên, chính vì điều đó, chúng ta thường khó tùy biến hay sử dụng lại nó, thậm chí khó kiểm soát lỗi của nó hơn. Với một câu lệnh phức tạp, chúng ta không biết được nó có lỗi hay không cho tới khi chạy chương trình hay debug (Mà chương trình đã chạy được rồi thì vẫn có lỗi tiềm ẩn :v chời đậu).

`Criteria API` thì ngược lại, nó cho phép chúng ta xây dựng câu lệnh một cách `Dynamic`, rất linh động, và không bị `hardcode` trong một `String` và có thể `tái sử dụng` lại được. Đặc biệt, vì là Java Object, nên chúng ta sẽ biết một câu lệnh bị lỗi, không đúng quy tắc ngay khi biên dịch chương trình rồi.

Túm váy lại, với một lệnh đơn giản như ví dụ đầu bài, thì các bạn nên xài `JPQL`, còn với những câu lệnh phức hợp, thay đổi theo `context` của chương trình thì nên sử dụng `Criteria`.

### **How to use.**

Quay trở lại với ví dụ ban đầu nhé:

```
CriteriaBuilder builder = em.getCriteriaBuilder();

CriteriaQuery<Office> query =  builder.createQuery(Office.class);
Root<Office> root = query.from(Office.class);
query.select(root);
```

Chúng ta cùng tìm hiểu từng dòng lệnh:

- `CriteriaBuilder`: Để xây dựng một câu query, các bạn sẽ cần tới `interface``CriteriaBuilder`, mục đích của nó là giúp tạo ra đối tượng chứa câu lệnh truy vấn `CriteriaQuery` và cung cấp cơ số các phép biến đổi, phép logic, điều kiện cho câu lệnh (and, or, not, avg, greater than,v.v...)
- `CriteriaQuery`: Đối tượng chính của chúng ta đây, nó được tạo ra bởi `builder.createQuery(Office.class)`. Mục đích là khai báo đối tượng bạn muốn lấy ra sau khi thực hiện query. Nó tương đương với đoạn ngoặc đơn ở dưới đây:

- `Root`: root là khai báo đối tượng bạn sẽ sử dụng trong query, tương đương với đối tượng sau mệnh đề `FROM`

Cuối cùng, để hoàn thiện câu lệnh `SELECT` chỉ đơn giản là lấy đối tượng `CriteriaQuery` đã khai báo là sử dụng function `select`. Đối tượng truyền vào chính là cái `root` (hay cái đối tượng của `FROM`) kia.

Trông vậy chứ cũng dễ dễ rồi đấy nhỉ :)))

Okie, có điều này không biết đã bạn nào để ý chưa 😅 chúng ta mới tạo ra câu lệnh, chứ chưa hề gọi nó xuống `Database` 😅

Để sử dụng câu lệnh đã tạo, các bạn làm giống với `JPQL` đó là sử dụng đối tượng `EntityManager`

```
TypedQuery<Office> query = em.createQuery(query);
List<Office> results = query.getResultList();
```

Oh right, thế là implement xong ví dụ đơn giản đâu tiên, không cóa gì khó khăn 🤔 (chém). Bây giờ thử advanced lên tý nhỉ:

Bây giờ mình muốn lấy tất cả `Office` ở thành phố `hanoi` thì sẽ làm như nào?

```
SELECT o FROM Office o WHERE o.city = 'hanoi'
```

Lúc này query của chúng ta sẽ như thế này:

```
query.select(root).where(builder.equal(root.get("city"), "hanoi"));
```

Các bạn để ý đoạn này nhé. Mình sử `builder` để lấy hàm `equal` (phép toán logic, như mình đề cập ở trên, chuẩn chưa nào). Tiếp tới là cái `root.get("city")`, `root` chính là đối tượng chúng ta đã khai báo, bây giờ chúng ta sẽ lấy trường `city` của nó và kiểm tra nó với `hanoi`.

Có thể nói `Criteria API` đã hiện thực hóa rất thành công câu lệnh JPQL (hay HQL) thành những api java cực kì dễ dọc, dễ hiểu và dễ sử dụng. Khi đã hiểu được câu lệnh gốc, bạn có thể dễ dàng chuyển nó thành `Criteria` và ngược lại.

Trong bài viết tới ở chương `Spring`, mình sẽ hướng dẫn các bạn sử dụng tới `Specifications` kết hợp `Criteria API` để tạo ra một vụ nổ khi giao tiếp với db bằng `Java` (vãi cả chém 😂)

Chúc các bạn sử dụng thành công `Criteria API` và đừng quên like or chia sẻ bài viết cho bạn bè hihi, cảm ơn các bạn đã theo dõi!

💁 Nếu có, toàn bộ project / code mẫu được lưu trữ tại **GitHub**

🌟 Đây là một bài viết trong Series **Làm chủ Spring Boot – Zero to Hero**

「Jpa」Hướng dẫn sử dụng Criteria API trong Hibernate (Phần 2)
===========================================================================

- Giới thiệu
- Cài đặt
- JPA Meta Model
- Predicate

### **Giới thiệu**

Trong bài viết trước, tôi đã giới thiệu với các bạn về **Criteria API** của **Hibernate**.

1. Hibernate là gì?
2. Hướng dẫn sử dụng Criteria API trong Hibernate

Trong phần này chúng ta sẽ tìm hiểu một số các phần cần thiết khác trong **Criteria API**, thứ giúp cho bạn xây dựng query một cách đơn giản hơn.

Trong bài có sử dụng:

1. Lombok

### **Cài đặt**

Nhớ thêm `spring-boot-starter-data-jpa` vào dependencies của bạn.

Trong phần này, tôi xài _H2 database_ để demo. (H2 là dạng memory database, nó sẽ lưu trong ram và khi tắt chương trình nó sẽ mất sạch)

```
<?xml version="1.0" encoding="UTF-8"?>
<projectxmlns="http://maven.apache.org/POM/4.0.0"xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"xsi:schemaLocation="http://maven.apache.org/POM/4.0.0https://maven.apache.org/xsd/maven-4.0.0.xsd"><modelVersion>4.0.0</modelVersion><parent><groupId>org.springframework.boot</groupId><artifactId>spring-boot-starter-parent</artifactId><version>2.0.5.RELEASE</version><relativePath/> <!-- lookup parent from repository -->
	</parent><groupId>me.loda.spring</groupId><artifactId>example-independent-maven-spring-project</artifactId><version>0.0.1-SNAPSHOT</version><name>example-independent-maven-spring-project</name><description>Demo project for Spring Boot</description><properties><java.version>1.8</java.version></properties><dependencies><dependency><groupId>org.springframework.boot</groupId><artifactId>spring-boot-starter-web</artifactId></dependency><dependency><groupId>org.springframework.boot</groupId><artifactId>spring-boot-devtools</artifactId><scope>runtime</scope><optional>true</optional></dependency><dependency><groupId>org.projectlombok</groupId><artifactId>lombok</artifactId><optional>true</optional></dependency><dependency><groupId>org.springframework.boot</groupId><artifactId>spring-boot-starter-test</artifactId><scope>test</scope><!--			<exclusions>-->
			<!--				<exclusion>-->
			<!--					<groupId>org.junit.vintage</groupId>-->
			<!--					<artifactId>junit-vintage-engine</artifactId>-->
			<!--				</exclusion>-->
			<!--			</exclusions>-->
		</dependency><!--spring jpa-->
		<dependency><groupId>org.springframework.boot</groupId><artifactId>spring-boot-starter-data-jpa</artifactId></dependency><!--in memory database-->
		<dependency><groupId>com.h2database</groupId><artifactId>h2</artifactId><scope>runtime</scope></dependency><!--https://mvnrepository.com/artifact/org.hibernate/hibernate-jpamodelgen -->
		<dependency><groupId>org.hibernate</groupId><artifactId>hibernate-jpamodelgen</artifactId><version>5.4.9.Final</version><scope>provided</scope></dependency></dependencies><build><plugins><plugin><groupId>org.springframework.boot</groupId><artifactId>spring-boot-maven-plugin</artifactId></plugin></plugins></build></project>
```

Đặc biệt, chú ý `hibernate-jpamodelgen`, tôi sẽ giải thích tác dụng của nó trước.

### **JPA Meta Model**

trong bài trước mọi người cũng biết cách cấu trúc của một câu query trong Criteria API:

```
SELECT o FROM Office o WHERE o.city = 'hanoi'
```

Lúc này query của chúng ta sẽ như thế này:

```
query.select(root).where(builder.equal(root.get("city"), "hanoi"));
```

Để ý thì có thể thấy, khi muốn lấy column `city` để kiểm tra, chúng ta đang hardcode bằng `String`.

Có một số bất lợi khi làm vậy, thứ nhất là bạn phải tự nhớ tên các column mỗi khi gọi, thứ hai là bạn sẽ phải tìm kiếm tất cả các chỗ sử dụng mỗi sửa đổi tên cột.

Cách giải quyết hay nhất là tham chiếu tên các column của **Table** vào một **Object** để chúng ta có thể gọi tới mỗi khi sử dụng. Khi có sự thay đổi, chỉ cần thay đổi trong đối tượng này là xong. Đối tượng biểu diễn này được gọi là **Meta Model**.

Và rất may là Hibernate hỗ trợ chúng ta tự động generate ra các Meta Model từ các class `@Entity`

Ví dụ:

Chúng ta có Class Entity `User`

_User.java_

```
@Entity
@Data
@Builder
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private UserType type;
    private String name;

    public enum UserType {
        NORMAL, VIP;
    }
}
```

Thì class Meta Model của `User` sẽ tên là `User_` và có cấu trúc như sau:

```
@Generated(value = "org.hibernate.jpamodelgen.JPAMetaModelEntityProcessor")
@StaticMetamodel(User.class)
public abstract class User_ {

	public static volatile SingularAttribute<User, String> name;
	public static volatile SingularAttribute<User, Long> id;
	public static volatile SingularAttribute<User, UserType> type;

	public static final String NAME = "name";
	public static final String ID = "id";
	public static final String TYPE = "type";

}
```

Để có thể Generate ra các class Meta Model, bạn sẽ cần thêm dependency `hibernate-jpamodelgen` vào project của mình.

```
<dependency><groupId>org.hibernate</groupId><artifactId>hibernate-jpamodelgen</artifactId><version>CURRENT-VERSION</version></dependency>
```

Khi build jar nó sẽ tự động generate thêm cho bạn

Để có thể sử dụng trong IDE, bạn có thể Config Annotation Processor (Giống với Lombok), để IDE hiểu và tự động generate ra giúp bạn lập trình dễ hơn.

!image

### **Predicate**

Để có thể xây dựng câu truy vấn một cách trọn vẹn, bạn cần biết `Predicate`.

Tạm hiểu một cách đơn giản thì `Predicate` là một mệnh đề điều kiện trong câu lệnh truy vấn.

Như ví dụ ở dưới đây:

```
@Repository
public class CustomUserRepository {

    @PersistenceContext
    private EntityManager em;

    public User getUserById(Long id) {
        CriteriaBuilder builder = em.getCriteriaBuilder();
        CriteriaQuery<User> query = builder.createQuery(User.class);
        Root<User> root = query.from(User.class);

        Predicate condition = builder.equal(root.get(User_.ID), id);

        query.select(root).where(condition);

        return em.createQuery(query).getSingleResult();
    }
}
```

`Predicate` có thể liên kết với nhau bằng các phép quan hệ `and`, `or`, `not`, v.v..

Ví dụ:

```
    public Collection<User> getUserByComplexConditions(String name, UserType type) {
        CriteriaBuilder builder = em.getCriteriaBuilder();
        CriteriaQuery<User> query = builder.createQuery(User.class);
        Root<User> root = query.from(User.class);

        Predicate hasNameLike = builder.like(root.get(User_.NAME), name);
        Predicate hasType = builder.equal(root.get(User_.TYPE), type);

        Predicate condition = builder.and(hasNameLike, hasType);

        query.select(root).where(condition);
        return em.createQuery(query).getResultList();
    }
```

Tới đây bạn đã nắm được cơ bản **Criteria API** và đã có thể tự sử dụng nó trong đa số các câu lệnh đơn giản.

Trong bài tiếp theo, tôi sẽ hướng dẫn nốt các join bảng nữa thôi là bạn sẽ mas con ông ter.

💁 Nếu có, toàn bộ project / code mẫu được lưu trữ tại **GitHub**

🌟 Đây là một bài viết trong Series **Làm chủ Spring Boot – Zero to Hero**

「Jpa」Hướng dẫn tự tạo Validator để kiểm tra Model & Entity
============================================================================

- Giới thiệu
- Cài đặt
- Prepare
- Tạo Annotation
- Tạo Validator
- Chạy thử
- Kết

### **Giới thiệu**

Bản thân Hibernate và Java đã cung cấp cho chúng ta rất nhiều các Annotation để validate dữ liệu của model.

Chẳng hạn như: `@@NotBlank`, `@Size`, `@Email`, v.v..

Tuy nhiên, trên thực tế, chúng ta có rất nhiều các điều kiện được đặt ra, tuỳ thuộc vào business và mô hình dự án.

Ví dụ như trong dự án của tôi, tôi muốn tất cả `User` đều phải có một thuộc tính là `lodaId`.

Một `lodaId` hợp lệ là chuỗi String có tiền tố: `loda://xxxx`

Lúc này, làm sao để tôi chắc chắn được rằng mọi `User` trước khi tạo đều phải có `lodaId` hợp lệ?

Rõ ràng tôi phải tự tạo ra một bộ kiểm tra cho riêng mình để kiểm soát tính hợp lệ của `User`.

Rất may, `hibernate-validator` sẽ giúp tôi làm điều đó.

Trong bài có sử dụng các kiến thức:

1. Spring Boot
2. jpa
3. lombok

### **Cài đặt**

_pom.xml_

```
<?xml version="1.0" encoding="UTF-8"?>
<projectxmlns="http://maven.apache.org/POM/4.0.0"xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"xsi:schemaLocation="http://maven.apache.org/POM/4.0.0https://maven.apache.org/xsd/maven-4.0.0.xsd"><modelVersion>4.0.0</modelVersion><parent><groupId>org.springframework.boot</groupId><artifactId>spring-boot-starter-parent</artifactId><version>2.0.5.RELEASE</version><relativePath/> <!-- lookup parent from repository -->
	</parent><groupId>me.loda.spring</groupId><artifactId>example-independent-maven-spring-project</artifactId><version>0.0.1-SNAPSHOT</version><name>example-independent-maven-spring-project</name><description>Demo project for Spring Boot</description><properties><java.version>1.8</java.version></properties><dependencies><dependency><groupId>org.springframework.boot</groupId><artifactId>spring-boot-starter-web</artifactId></dependency><dependency><groupId>org.springframework.boot</groupId><artifactId>spring-boot-devtools</artifactId><scope>runtime</scope><optional>true</optional></dependency><dependency><groupId>org.projectlombok</groupId><artifactId>lombok</artifactId><optional>true</optional></dependency><dependency><groupId>org.springframework.boot</groupId><artifactId>spring-boot-starter-test</artifactId><scope>test</scope></dependency><!--https://mvnrepository.com/artifact/org.hibernate/hibernate-validator -->
        <dependency><groupId>org.hibernate</groupId><artifactId>hibernate-validator</artifactId><version>6.1.0.Final</version></dependency></dependencies><build><plugins><plugin><groupId>org.springframework.boot</groupId><artifactId>spring-boot-maven-plugin</artifactId></plugin></plugins></build></project>
```

### **Prepare**

Trước khi tạo ra một bộ Validator cho riêng mình, chúng ta tạo ra các thành phần chính.

Tạo ra model `User`

_User.java_

```
@Data
public class User {
    private String lodaId;
}
```

Tạo Controller giao tiếp với client.

_UserController.java_

```
@RestController
@RequestMapping("api/v1/users")
public class UserController {

    /*
        Đánh dấu object với @Valid để validator tự động kiểm tra object đó có hợp lệ hay không
     */
    @PostMapping
    public Object createUser(@Valid @RequestBody User user) {
        return user;
    }

}
```

### **Tạo Annotation**

Để tạo ra một validator với Hibernate-Validator, chúng ta cần khai báo một annotation mới.

Ở đây, tôi tạo ra một Annotation của chính mình như sau:

```
/**
 * Khai báo một custom annotation
 */
@Documented
@Constraint(validatedBy = LodaIdValidator.class)
@Target({ElementType.METHOD, ElementType.FIELD})
@Retention(RetentionPolicy.RUNTIME)
public @interface LodaId {
    // trường message là bắt buộc, khai báo nội dung sẽ trả về khi field k hợp lệ
    String message() default "LodaId must start with loda://";
    // Cái này là bắt buộc phải có để Hibernate Validator có thể hoạt động
    Class<?>[] groups() default {};
    // Cái này là bắt buộc phải có để Hibernate Validator có thể hoạt động
    Class<? extends Payload>[] payload() default {};
}
```

Vậy là xong, giờ chỉ cần gắn `@LodaId` lên trường nào cần kiểm tra tính hợp lệ.

```
@Data
public class User {
    // Đánh dấu field lodaId sẽ cần validate bởi @LodaId
    @LodaId
    private String lodaId;
}
```

### **Tạo Validator**

Sau khi đã tạo ra `@LodaId`, chúng ta cần tạo ra bộ kiểm tra cho Annotation này.

Rất đơn giản, bạn chỉ cần kế thừa lớp `ConstraintValidator` của Hibernate Validator

```
import javax.validation.ConstraintValidator;
import javax.validation.ConstraintValidatorContext;

public class LodaIdValidator implements ConstraintValidator<LodaId, String> {
    private static final String LODA_PREFIX = "loda://";

    /**
     * Kiểm tra tính hợp lệ của trường được đánh dấu bởi @LodaId
     * @param s
     * @param constraintValidatorContext
     * @return
     */
    @Override
    public boolean isValid(String s, ConstraintValidatorContext constraintValidatorContext) {
        if (s == null || s.isEmpty()) return false;

        return s.startsWith(LODA_PREFIX);
    }
}
```

### **Chạy thử**

Request một `User` hợp lệ:

```
{
	"lodaId": "loda://user_1"
}
```

trả về thành công `200`.

!image

Request một `User` không hợp lệ:

```
{
	"lodaId": "Laula://user_1"
}
```

Trả về thất bại, Bad Request `400`.

!image

```
{
    "timestamp": "2019-12-19T10:26:14.554+0000",
    "status": 400,
    "error": "Bad Request",
    "errors": [
        {
            "codes": [
                "LodaId.user.lodaId",
                "LodaId.lodaId",
                "LodaId.java.lang.String",
                "LodaId"
            ],
            "arguments": [
                {
                    "codes": [
                        "user.lodaId",
                        "lodaId"
                    ],
                    "arguments": null,
                    "defaultMessage": "lodaId",
                    "code": "lodaId"
                }
            ],
            "defaultMessage": "LodaId must start with loda://",
            "objectName": "user",
            "field": "lodaId",
            "rejectedValue": "Laula://user_1",
            "bindingFailure": false,
            "code": "LodaId"
        }
    ],
    "message": "Validation failed for object='user'. Error count: 1",
    "path": "/api/v1/users"
}
```

### **Kết**

Rất đơn giản để có thể tự tạo ra cho mình các Validator với `Hibernate-Validator`. Nó sẽ giúp bạn tiết kiệm rất nhiều thời gian.

💁 Nếu có, toàn bộ project / code mẫu được lưu trữ tại **GitHub**

🌟 Đây là một bài viết trong Series **Làm chủ Spring Boot – Zero to Hero**

