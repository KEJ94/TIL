# @ControllerAdvice & @ExceptionHandler
```@ControllerAdvice``` 는 ```@Controller``` 어노테이션이 있는 __모든 곳에서의 예외__ 를 잡을 수 있다.  
```@ControllerAdvice``` 안에 있는 ```@ExceptionHandler``` 는 모든 컨트롤러에서 발생하는 예외상황을 잡을 수 있고, ```@ControllerAdvice``` 의 속성 설정을 통하여 
원하는 컨트롤러나 패키지만 선택할수도 있다.  
따로 지정을 하지 않으면 모든 패키지에 있는 컨트롤러를 담당하게 된다.  

### ```@ControllerAdvice``` 적용
```java
package com.jiransnc.vada.exception;

import org.springframework.web.bind.annotation.ExceptionHandler;
import org.springframework.web.bind.annotation.RestControllerAdvice;
import com.jiransnc.vada.model.common.SimpleResponseData;
import lombok.extern.slf4j.Slf4j;

@Slf4j
@RestControllerAdvice
public class VADAExceptionHandler {
    @ExceptionHandler(VADAException.class)
    public SimpleResponseData vadaExceptionHandle(VADAException ve) {
        SimpleResponseData responseData = new SimpleResponseData();
        log.error(ve.getMessage(), ve);
        responseData.setStatus(ve.getReturnCode());
        return responseData;
    }
    
    @ExceptionHandler(Exception.class)
    public SimpleResponseData exceptionHandle(Exception e) {
        SimpleResponseData responseData = new SimpleResponseData();
        log.error(e.getMessage(), e);
        responseData.setStatus(VADAReturnCode.EXCEPTION_OCCURRED);
        return responseData;
    }
}
```
> 코드상에는 ```@RestControllerAdvice``` 를 사용했다. ```@ControllerAdvice``` 와 동일한 역할을 하지만 __객체를 반환할 수 있다__ 는 차이점이 있다.  
> 새로운 클래스 파일을 만들어 사용하면 된다.  
### ```@Controller``` ```try/catch``` 제거
```java
@PostMapping("/assets")
public ResponseData<List<Asset>> assetLogicalList(HttpSession session, @RequestBody RequestData reqData){
    ResponseData<List<Asset>> responseData = new ResponseData<List<Asset>>();
    try {
        reqData.setFilter();
        List<Asset> assets = assetFindService.findAssetLogical(reqData);
        Paging paging = reqData.getPaging();
        int totalCount = assetFindService.findAssetLogicalCount(reqData);
        paging = paging.calcStartEndPage(assets.size(), totalCount);
        responseData.setResponseData(assets, paging);
    } catch(VADAException ve){
        ve.printStackTrace();
        responseData.setStatus(ve.getReturnCode());
    } catch(Exception e){
        e.printStackTrace();
        responseData.setStatus(VADAReturnCode.EXCEPTION_OCCURRED);
    }
    return responseData;
}
```
```java
@PostMapping("/assets")
public ResponseData<List<Asset>> assetLogicalList(HttpSession session, @RequestBody RequestData reqData) throws Exception  {
    ResponseData<List<Asset>> responseData = new ResponseData<List<Asset>>();
    reqData.setFilter();
    List<Asset> assets = assetFindService.findAssetLogical(reqData);
    Paging paging = reqData.getPaging();
    int totalCount = assetFindService.findAssetLogicalCount(reqData);
    paging = paging.calcStartEndPage(assets.size(), totalCount);
    responseData.setResponseData(assets, paging);
    return responseData;
}
```
> 컨트롤러 마다 들어간 같은 예외처리문을 통합해서 한곳으로 관리할 수 있다
