# FIND_IN_SET 
문법 ```WHERE FIND_IN_SET(str, strlist)```
```sql
SELECT * FROM vw_asset WHERE FIND_IN_SET(18, id)
```
> id 의 { 18, 57 }, { 18, 15 }, { 18 } 등의 리스트가 출력된다.  
> id 의 값은 자식테이블의 아이디를 ```GROUP_CONCAT``` 한 구조다.
