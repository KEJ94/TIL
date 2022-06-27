# 래퍼 클래스 - WrapperClass
자바의 자료형은 크게 __기본타입__, __참조타입__ 으로 나눠진다.

### 기본타입(PrimitiveType) - 참조타입(WrapperClass)
- byte - Byte
- char - Character
- int - Integer
- float - Float
- double - Double
- boolean - Boolean
- long - Long
- short - Short


### Boxing (기본 타입의 값을 포장 객체로 만드는 과정)
```
int num = 10;
Integer boxing = new Integer(num)
```

### UnBoxing (포장객체에서 기본타입의 값을 얻어내는 과정)
```
Integer num = new Integer(10);
int unboxing = num.intValue();
```
### AutoBoxing & AutoUnBoxing (JDK 1.5 부터)
```
Integer a = new Integer(10);
int b = a;
Integer c = b
```

### WrapperClass 구조
![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fk.kakaocdn.net%2Fdn%2FZolLT%2Fbtq8R0SHB75%2FIfszu8aary6ZEM8Jo6tANK%2Fimg.png)  

### 문자열을 기본 타입 값으로 반환
```
String str = "10";
String str2 = "10.5";
String str3 = "true";
        
byte b = Byte.parseByte(str);
int i = Integer.parseInt(str);
short s = Short.parseShort(str);
long l = Long.parseLong(str);
float f = Float.parseFloat(str2);
double d = Double.parseDouble(str2);
boolean bool = Boolean.parseBoolean(str3);
```
### 값 비교
```
Integer num = new Integer(10); //래퍼 클래스1
Integer num2 = new Integer(10); //래퍼 클래스2
int i = 10; //기본타입
		 
System.out.println("래퍼클래스 == 기본타입 : "+(num == i)); //true
System.out.println("래퍼클래스.equals(기본타입) : "+num.equals(i)); //true
System.out.println("래퍼클래스 == 래퍼클래스 : "+(num == num2)); //false
System.out.println("래퍼클래스.equals(래퍼클래스) : "+num.equals(num2)); //true
```
### 사용하는 이유
- 기본 데이터 타입을 Object로 변환할 수 있다.
- java.util 패키지의 클래스는 객체만 처리하므로 WrapperClass는 이 경우에도 도움이 된다.
- 컬렉션 프레임워크의 데이터 구조는 기본 타입이 아닌 객체만 저장하게 되고, WrapperClass를 사용하여 AutoBoxing/AutoUnBoxing
<br><br><br>
