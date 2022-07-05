## JSON 
### ```JSON.stringify``` to ```List<HashMap<String, String>>```
- Excel 파일 출력을 위해 POST 메서드로 요청해야 하는 상황
- ajax 로 처리하기 번거로움
- submit 으로 요청하려는데 ```@RequestBody``` 어노테이션을 사용하여 맵핑 불가 (404)
- 아래 코드로 header object 파싱
```
var params = {"header" : JSON.stringify(header)};
```
```
ObjectMapper objectMapper = new ObjectMapper();
List<HashMap<String,String>> readValue = objectMapper.readValue(header, new TypeReference<List<HashMap<String,String>>>() {});
```
