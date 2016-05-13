---
layout: post
title: Spring AOP Annotation Pointcut
---

### AopTestApplication.java
```java
package plumhj.aop.test;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.EnableAspectJAutoProxy;

@SpringBootApplication
@EnableAspectJAutoProxy
public class AopTestApplication {

	public static void main(String[] args) {
		SpringApplication.run(AopTestApplication.class, args);
	}

	@Autowired
	AspectA a;
	
	@Bean
	public AspectA getAspectA(){
		return new AspectA();
	}
}
```

### AspectA.java
```java
package plumhj.aop.test;

import org.aspectj.lang.ProceedingJoinPoint;
import org.aspectj.lang.annotation.Around;
import org.aspectj.lang.annotation.Aspect;
import org.aspectj.lang.annotation.Pointcut;

@Aspect
public class AspectA {

	@Pointcut("@annotation(org.springframework.web.bind.annotation.RequestMapping)")
    public void requestMapping() {}

	@Around("requestMapping()")
	public Object auth(ProceedingJoinPoint pjp) throws Throwable {
        String name = pjp.getSignature().getName();
        System.out.println("before : " + name);
        System.out.println("args : " + pjp.getArgs()[0].toString());
        System.out.println("args : " + pjp.getArgs()[1].toString());
        
        Object ret = pjp.proceed();
        System.out.println("after : " + ret.toString());
        return "hi";
    }	
}
```

### TestController.java
```java
package plumhj.aop.test;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class TestController {
	
	@RequestMapping(value = "/", method = { RequestMethod.GET })
	public String root(@RequestParam("id") String id, @RequestParam("password") String password){
		return "Hello world!!";
	}
	
	
}
```
****
