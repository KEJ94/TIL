# Foreign Keys (외래키) 할당
테이블 확인
```mysql
SELECT TABLE_SCHEMA, TABLE_NAME, COLUMN_NAME
FROM INFORMATION_SCHEMA.COLUMNS
WHERE COLUMN_NAME='컬럼명';
```
확인한 테이블에 외래키 할당
```
이름 : fk_현재테이블명_원본테이블명_현재참조컬럼
  └  현재참조컬럼
소유자 : 현재 테이블  
참조 테이블 : 원본 테이블  
Type : FOREIGN KEY  
On Delete : Restrict (삭제 X)
On Update : Restrict (업데이트 X)
```



