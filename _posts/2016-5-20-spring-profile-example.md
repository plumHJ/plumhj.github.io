---
layout: post
title: "[Spring] Profile Annotation Example"
---

Profile 어노테이션을 활용한 의존성 주입 예제

### ProfileApplicationTests.java

```java
import static org.junit.Assert.assertEquals;

import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.test.context.junit4.SpringRunner;

@RunWith(SpringRunner.class)
@SpringBootTest
public class ProfileApplicationTests {

	@Autowired
	Animal animal;
	
	@Test
	public void test() {
		assertEquals("멍멍~", animal.say());
	}

}
```

### Animal.java

```java
package plumhj.springboot.examples;

public interface Animal {

	public String say();
	
}
```

### Dog.java

```java
package plumhj.springboot.examples;

public class Dog implements Animal {

	@Override
	public String say() {
		return "멍멍~";
	}

}
```

### Human.java

```java
package plumhj.springboot.examples;

public class Human  implements Animal {

	@Override
	public String say() {
		return "Show me the money~!!";
	}

}
```

### AnimalConfig.java

```java
package plumhj.springboot.examples;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.Profile;

@Configuration
public class AnimalConfig {
	
	@Bean
	@Profile("human")
	public Animal human(){
		return new Human();
	}
	
	@Bean
	@Profile("dog")
	public Animal dog(){
		return new Dog();
	}
	
}
```

### application.properties
```
spring.profiles.active=dog
```


****
