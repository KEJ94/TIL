# Custom ```@Valid``` Annotation
스프링에서 제공되는 ```@Valid``` 어노테이션에서 제공하지 않는 Valid 처리를 하고싶을때 아래와 같은 코드로 작성한다. 
### 커스텀 어노테이션 만들기
```java
@Target(ElementType.FIELD) // 1
@Retention(RetentionPolicy.RUNTIME) // 2
@Constraint(validatedBy = AssetTypeValidator.class) // 3
public @interface AssetType {
  String message() default "자산 타입"; // 4
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
public class AssetTypeValidator implements ConstraintValidator<AssetType, String> { // 1, 2

  @Override
  public boolean isValid(String value, ConstraintValidatorContext context) { // 3
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
> Controller 또는 Service 등 필요한 영역에  ```@Valid``` 어노테이션을 추가해서 사용한다.
