## GraphQL 이란?
facebook에서 만든 API를 위한 쿼리 언어이며, 특정한 데이터베이스나 스토리지에 귀속되어 있지 않으며 기존 코드와 데이터에 의해 대체된다.    
기존에 사용하고 있는 REST API의 한계점을 극복하고자 나온 통신 규약으로 REST API를 대체할 수 있다.  
### 공식 문서  
https://graphql.org/  
https://graphql-kr.github.io/
<br><br>
## REST API 의 한계
복잡한 서비스나 클라이언트의 요청사항에 따라 __Over-Fetching__ 과 __Under-Fetching__ 이 발생한다.  
<br>  
### Over-Fetching
예를 들어, 사용자의 데이터를 조회하는 /user/ API가 있다고 가정한다면, __사용자 번호 : 1__ 에 해당하는 데이터를 조회한다면 아래와 같은 형태가 된다.  
```
GET /user/1/
response body 
{
 "user_no": 1,
 "user_name": "김응주",
 "usere_grade": "VIP",
 "zip": "11053",
 "last_login_timestamp": "2022-06-25 19:07:01",
 ...
}
```
/user/1/ API를 호출한 다음 user_name 만 응답받고 싶은 상황에서, 불필요한 user_grade, zip 등의 사용하지 않는 데이터도 같이 반환받는다.  
이는 곧 리소스의 낭비라고 볼 수 있고 이와 같은 낭비를 __Over-Fetching__ 이라 한다.  
<br>
### Under-Fetching  
쇼핑몰 서비스의 경우, 로그인한 사용자의 장바구니 정보를 보여준다고 가정하면 여러 API를 호출하게 된다.
```
/user/1/
/cart/
/notification/
/wish/
...
```
요청에 맞게 유효한 데이터를 보여주기 위해 API를 여러번 호출하게 되는 경우를 __Under-Fetching__ 이라 한다.  
#### REST API 목록  
```
/item/
/item/detail/
/item/image/
/item/notice/
/item/manage/
...
```  
REST API 방법론으로 API를 만든다고 하면 Resource 별로 Endpoint를 갖기 때문에 비슷하지만 다른 API를 생성하게 된다.  
이는 서비스가 커져갈때 관리 포인트가 늘어나게 되므로 개발자나 클라이언트에게 부담이 된다.  
<br><br>
## GraphQL 사용
GraphQL은 위에서 설명한 것처럼 REST API의 한계를 극복하고자 나왔다.  
Endpoint는 통상 1개만 생성하고 클라이언트에게 필요한 데이터는 클라이언트가 직접 쿼리를 작성, 호출하여 반환 받는다.  
### Over-Fetching 문제 해결  
요청 쿼리
```
query {  
 user(user_no: 1) {  
 user_name  
 }  
}  
```
반환 데이터  
```
{  
  "data": {  
    "user": {  
      "user_name": "김응주",  
    }  
  }  
}  
```
### Under-Fetching 문제 해결  
요청 쿼리  
```
query {
 cart {
 product_name
 price
 }
 notification {
 is_read
 }
 user(user_id: 1) {
 user_name
 user_grade
 }
}  
```
반환 데이터  
```
{
  "cart": [{
    "product_name": "shoes",
    "price": 12000
  }],
  "notifications": [{
    is_read: true
  }],
  "user": {
    "user_name": "김응주",
    "user_grade": "VIP"
  }
}
```  
<br><br>
## 마무리  
### GraphQL 장점  
- 클라이언트가 필요한 데이터만 반환할 수 있음  
- 1번의 호출로 원하는 데이터를 한번에 가져올 수 있음  
- 대부분의 언어에서 라이브러리로 제공함  
### GraphQL 단점  
- 백엔드 개발자, 클라이언트 개발자 양쪽 다 러닝커브가 있음  
- 단순한 서비스에서는 사용하기가 복잡함  
- 캐싱 기능의 구현이 복잡함  
- 요청이 text로 날라가기 때문에 File 전송 등을 구현하기가 어려움  
### 요약  
GraphQL은 REST API의 한계점을 해결하고자 나온 통신 규약이다.       
__하지만 무조건 GraphQL이 REST API보다 낫다는 것이 아니므로__ 서비스 별로 잘 판단하여 레이어를 선택하는 것이 중요하다.  
<br><br><br><br>    
