---
layout: post
title: "[Spring] AOP Annotation Pointcut"
---

AOP를 활용한 requestMapping Annotation의 인증제어

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
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;

@Aspect
public class AspectA {

	@Pointcut("@annotation(org.springframework.web.bind.annotation.RequestMapping)")
    public void requestMapping() {}

	@Around("requestMapping()")
	public Object auth(ProceedingJoinPoint pjp) throws Throwable {
        String name = pjp.getSignature().getName();
        System.out.println("method : " + name);
        
        String id = pjp.getArgs()[0].toString();
        String password = pjp.getArgs()[1].toString();
        
        System.out.println("id : " + id);
        System.out.println("password : " + password);
        
        if(id.equals("root") && password.equals("1234")){
        	return pjp.proceed();
        }else{
        	return new ResponseEntity<String>("You are not authorized", HttpStatus.BAD_REQUEST); 
        }
        
    }
	
}
```

### TestController.java

```java
package plumhj.aop.test;

import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class TestController {
	
	@RequestMapping(value = "/", method = { RequestMethod.GET })
	public ResponseEntity<String> root(@RequestParam("id") String id, @RequestParam("password") String password){
		return new ResponseEntity<String>("Hello! " + id, HttpStatus.OK);
	}
}
```

****
