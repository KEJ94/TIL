# Spring Cache
값의 변화가 자주 일어나지 않는 GROUP BY 데이터를 어플리케이션이 실행될때 한번 DB 에서 추출한 다음 메모리에 저장 후 꺼내서 사용  

__build.gradle__
```
implementation 'org.springframework.boot:spring-boot-starter-cache'
implementation 'net.sf.ehcache:ehcache'
```
__Application.java__
```java
@EnableCaching // 추가 
@EnableAsync
@SpringBootApplication
public class VadaApplication {
```
> 특정 영역에서만 지정할수도 있지만 지금은 어플리케이션 전역에서 사용한다고 가정한다.

__ServletListener.java__
```java
package com.jiransnc.vada.interceptor;

import java.util.ArrayList;
import java.util.List;

import javax.servlet.ServletContextEvent;
import javax.servlet.ServletContextListener;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.cache.Cache;
import org.springframework.cache.CacheManager;
import org.springframework.stereotype.Component;

// import com.jiransnc.vada.service.cache.CacheService;

@Component
public class VadaServletListener implements ServletContextListener {

    @Autowired
    //private CacheService cacheService;
    private CacheManager cacheManager;

    public VadaServletListener(CacheManager cacheManager) {
        this.cacheManager = cacheManager;
    }

    @Override
    public void contextInitialized(ServletContextEvent sce) {
        Cache cache = cacheManager.getCache("asset");
        List<String> platformType = new ArrayList<String>();
        cache.put("platformType", platformType);
    }

    @Override
    public void contextDestroyed(ServletContextEvent sce) {

    }
}
```
> tomcat 이 실행될때 데이터를 저장한다.

__CacheController.java__
```java
package com.jiransnc.vada.controller.cache;

import java.util.HashMap;

import org.springframework.cache.Cache;
import org.springframework.cache.CacheManager;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
@RequestMapping("/api")
public class CacheController {
    
    private CacheManager cacheManager;

    public CacheController(CacheManager cacheManager){
        this.cacheManager = cacheManager;
    }

    @GetMapping("/cache")
    public HashMap<String, Object> assetGroupList() throws Exception{
        HashMap<String, Object> cacheMap = new HashMap<String, Object>();
        Cache cache = cacheManager.getCache("asset");
        cacheMap.put("platformType", cache.get("platformType").get());
        return cacheMap;
    }
}
```
> 컨트롤러에서 DB 커넥션 없이 캐시데이터를 get 해서 사용한다.
