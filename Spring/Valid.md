# Custom ```@Valid``` Annotation
스프링에서 제공되는 ```@Valid``` 어노테이션에서 제공하지 않는 Valid 처리를 하고싶을때 아래와 같은 코드로 작성한다. 
### 커스텀 어노테이션 만들기
```java
package com.jiransnc.vada.utils.annotation;

import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;
import javax.validation.Constraint;
import com.jiransnc.vada.utils.annotation.validator.AssetTypeValidator;

@Target(ElementType.FIELD) // 1
@Retention(RetentionPolicy.RUNTIME) // 2
@Constraint(validatedBy = AssetTypeValidator.class) // 3
public @interface AssetType {
  String message() default "TEST"; // 4 오류 메세지 default
  Class[] groups() default {};
  Class[] payload() default {};
}
```
> 1. 변수 위에 사용하는 어노테이션이기 때문에 Target 은 ```FIELD``` 
> 2. 어노테이션 유지범위는 실행하는 동안으로 설정해주기 위해 Retention 을 ```RUNTIME``` 
> 3. 검증 클래스를 지정해주기 위해 ```validatedBy``` 로 클래스를 지정
> 4. 기본적인 메세지를 설정할 수 있다. (```grpups```, ```payload``` 설정은 생략)  
### 검증 클래스 만들기  
```java
package com.jiransnc.vada.utils.annotation.validator;
import javax.validation.ConstraintValidator;
import javax.validation.ConstraintValidatorContext;
import com.jiransnc.vada.mapper.AssetMapper;
import com.jiransnc.vada.utils.annotation.AssetType;

public class AssetTypeValidator implements ConstraintValidator<AssetType, String> {

    AssetMapper dbMapper;

    @Override
    public boolean isValid(String value, ConstraintValidatorContext context) {
        if (value == null) {
            return false;
        }
        return value.matches("(01[016789])(\\d{3,4})(\\d{4})");
    }
}
```
> 1. 검증 클래스를 만들기 위해서는 ```ConstraintValidator``` 를 구현해주어야 한다.  
> 2. __첫번째 제네릭 값으로는 검증 어노테이션__ , __두번째 제네릭 값으로는 필드의 데이터 유형__ 을 넣어준다.
> 3. ```isValid``` 를 구현해서 값을 검증할 로직을 넣어준다.
### 필드에 어노테이션 추가 
```java
@Getter
public class Asset {

  @AssetType
  private String assetType;
}
```
> 필드에 ```@Valid``` 어노테이션을 추가한다.  
### 컨트롤러에 적용
```java
@PostMapping("/assets/add")
public SimpleResponseData assetAdd(@RequestBody @Valid Asset asset){ // @Valid
    SimpleResponseData responseData = new SimpleResponseData();
    try {
        assetAddService.addAsset(asset);
    } catch(VADAException ve){
        log.error(ve.getMessage(), ve);
        responseData.setStatus(ve.getReturnCode());
    }catch (Exception e) {
        log.error(e.getMessage(), e);
        responseData.setStatus(VADAReturnCode.EXCEPTION_OCCURRED);
    }
    return responseData;
}
```
> 컨트롤러에 ```@Valid``` 어노테이션을 추가한다.
### 유효성 검증 실패 Response
```json
{"timestamp":"2023-03-21T01:05:19.887+00:00","status":400,"error":"Bad Request","trace":"org.springframework.web.bind.MethodArgumentNotValidException: Validation failed for argument [0] in public com.jiransnc.vada.model.common.SimpleResponseData com.jiransnc.vada.controller.asset.AssetController.assetAdd(com.jiransnc.vada.dao.asset.Asset): [Field error in object 'asset' on field 'name': rejected value [null]; codes [AssetType.asset.name,AssetType.name,AssetType.java.lang.String,AssetType]; arguments [org.springframework.context.support.DefaultMessageSourceResolvable: codes [asset.name,name]; arguments []; default message [name]]; default message [TEST]] \r\n\tat org.springframework.web.servlet.mvc.method.annotation.RequestResponseBodyMethodProcessor.resolveArgument(RequestResponseBodyMethodProcessor.java:141)\r\n\tat org.springframework.web.method.support.HandlerMethodArgumentResolverComposite.resolveArgument(HandlerMethodArgumentResolverComposite.java:122)\r\n\tat org.springframework.web.method.support.InvocableHandlerMethod.getMethodArgumentValues(InvocableHandlerMethod.java:179)\r\n\tat org.springframework.web.method.support.InvocableHandlerMethod.invokeForRequest(InvocableHandlerMethod.java:146)\r\n\tat org.springframework.web.servlet.mvc.method.annotation.ServletInvocableHandlerMethod.invokeAndHandle(ServletInvocableHandlerMethod.java:117)\r\n\tat org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter.invokeHandlerMethod(RequestMappingHandlerAdapter.java:895)\r\n\tat org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter.handleInternal(RequestMappingHandlerAdapter.java:808)\r\n\tat org.springframework.web.servlet.mvc.method.AbstractHandlerMethodAdapter.handle(AbstractHandlerMethodAdapter.java:87)\r\n\tat org.springframework.web.servlet.DispatcherServlet.doDispatch(DispatcherServlet.java:1071)\r\n\tat org.springframework.web.servlet.DispatcherServlet.doService(DispatcherServlet.java:964)\r\n\tat org.springframework.web.servlet.FrameworkServlet.processRequest(FrameworkServlet.java:1006)\r\n\tat org.springframework.web.servlet.FrameworkServlet.doPost(FrameworkServlet.java:909)\r\n\tat javax.servlet.http.HttpServlet.service(HttpServlet.java:696)\r\n\tat org.springframework.web.servlet.FrameworkServlet.service(FrameworkServlet.java:883)\r\n\tat javax.servlet.http.HttpServlet.service(HttpServlet.java:779)\r\n\tat org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:227)\r\n\tat org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:162)\r\n\tat org.apache.tomcat.websocket.server.WsFilter.doFilter(WsFilter.java:53)\r\n\tat org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:189)\r\n\tat org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:162)\r\n\tat org.springframework.web.filter.RequestContextFilter.doFilterInternal(RequestContextFilter.java:100)\r\n\tat org.springframework.web.filter.OncePerRequestFilter.doFilter(OncePerRequestFilter.java:117)\r\n\tat org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:189)\r\n\tat org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:162)\r\n\tat org.springframework.web.filter.FormContentFilter.doFilterInternal(FormContentFilter.java:93)\r\n\tat org.springframework.web.filter.OncePerRequestFilter.doFilter(OncePerRequestFilter.java:117)\r\n\tat org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:189)\r\n\tat org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:162)\r\n\tat org.springframework.web.filter.CharacterEncodingFilter.doFilterInternal(CharacterEncodingFilter.java:201)\r\n\tat org.springframework.web.filter.OncePerRequestFilter.doFilter(OncePerRequestFilter.java:117)\r\n\tat org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:189)\r\n\tat org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:162)\r\n\tat org.apache.catalina.core.StandardWrapperValve.invoke(StandardWrapperValve.java:177)\r\n\tat org.apache.catalina.core.StandardContextValve.invoke(StandardContextValve.java:97)\r\n\tat org.apache.catalina.authenticator.AuthenticatorBase.invoke(AuthenticatorBase.java:541)\r\n\tat org.apache.catalina.core.StandardHostValve.invoke(StandardHostValve.java:135)\r\n\tat org.apache.catalina.valves.ErrorReportValve.invoke(ErrorReportValve.java:92)\r\n\tat org.apache.catalina.core.StandardEngineValve.invoke(StandardEngineValve.java:78)\r\n\tat org.apache.catalina.connector.CoyoteAdapter.service(CoyoteAdapter.java:360)\r\n\tat org.apache.coyote.http11.Http11Processor.service(Http11Processor.java:399)\r\n\tat org.apache.coyote.AbstractProcessorLight.process(AbstractProcessorLight.java:65)\r\n\tat org.apache.coyote.AbstractProtocol$ConnectionHandler.process(AbstractProtocol.java:891)\r\n\tat org.apache.tomcat.util.net.NioEndpoint$SocketProcessor.doRun(NioEndpoint.java:1784)\r\n\tat org.apache.tomcat.util.net.SocketProcessorBase.run(SocketProcessorBase.java:49)\r\n\tat org.apache.tomcat.util.threads.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1191)\r\n\tat org.apache.tomcat.util.threads.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:659)\r\n\tat org.apache.tomcat.util.threads.TaskThread$WrappingRunnable.run(TaskThread.java:61)\r\n\tat java.base/java.lang.Thread.run(Thread.java:833)\r\n","message":"Validation failed for object='asset'. Error count: 1","errors":[{"codes":["AssetType.asset.name","AssetType.name","AssetType.java.lang.String","AssetType"],"arguments":[{"codes":["asset.name","name"],"arguments":null,"defaultMessage":"name","code":"name"}],"defaultMessage":"TEST","objectName":"asset","field":"name","rejectedValue":null,"bindingFailure":false,"code":"AssetType"}],"path":"/api/assets/add"}
```
> defaultMessage 로 아까 작성한 "TEST" 를 리턴 받는다.
### Exception Handling
에러 로그를 확인해보면 ```MethodArgumentNotValidException``` 이 발생했음을 알 수 있다. ErrorMessage 를 원하는 형태로 내보내기 위해서는 ```@ControllerAdvice``` 를 이용한 __전역 에러 핸들링__ 또는 ```@Controller``` 단에서 __지역 에러 핸들링__ 을 사용하면 된다.  
아래 코드는 ```@ControllerAdvice``` 를 이용한 전역 에러 핸들링 코드다. 
```java
@RestControllerAdvice
public class ApiControllerAdvice {

    @ExceptionHandler(MethodArgumentNotValidException.class)
    public SimpleResponseData methodArgumentNotValidExceptionHandle(MethodArgumentNotValidException e) {
        ValidationResponseData responseData = new ValidationResponseData();
        log.error(e.getMessage(), e);
        responseData.setStatus(VADAReturnCode.EXCEPTION_OCCURRED);
        HashMap<String, String> data = new HashMap<String, String>();
        e.getBindingResult().getFieldErrors()
                .forEach(c -> data.put(c.getField(), c.getDefaultMessage()));
        responseData.setData(data);
        return responseData;
    }
}
```
