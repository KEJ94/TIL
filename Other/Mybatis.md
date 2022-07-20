## Mybatis
마이바티스는 자바 퍼시스턴스 프레임워크의 하나로 XML 서술자나 애너테이션을 사용하여 저장 프로시저나 SQL 문으로 객체들을 연결시킨다.  
마이바티스는 아파치 라이선스 2.0으로 배포되는 자유 소프트웨어이다.  
<br><br>
## choose, when, otherwise
switch case 문과 비슷하다고 보면된다.
```
<choose>
	<when test="조건식1"> 쿼리문1 </when>
    	<when test="조건식2"> 쿼리문2 </when>
    	<when test="조건식3"> 쿼리문3 </when>
    	<when test="조건식4"> 쿼리문4 </when>
	<otherwise> 쿼리문5 </otherwise>
</choose>  
```
if문과 다른점은 if문은 조건식이 둘다 true 면 쿼리문 2개를 실행한다.
```
<select id="findActiveBlogLike" resultType="Blog">
  SELECT * FROM BLOG WHERE state = ‘ACTIVE’
  <choose>
 	 <when test="title != null">
    		AND title like #{title}    
   	 </when>
 	 <when test=test="author != null and author.name != null">
  	  	AND author_name like #{author.name}    
  	 </when>    
  </choose>
</select>
```
<br><br>

