# java.lang.OutOfMemoryError  
자바에서 힙 메모리(heap memory) 가 부족할 때 발생하는 에러  
톰캣은 실행중인 상태에서 애플리케이션만 해당 에러로 멈춘상태에서 발견됨  
```.sh
# resolve links - $0 may be a softlink
PRG="$0"

while [ -h "$PRG" ]; do
  ls=`ls -ld "$PRG"`
  link=`expr "$ls" : '.*-> \(.*\)$'`
  if expr "$link" : '/.*' > /dev/null; then
    PRG="$link"
  else
    PRG=`dirname "$PRG"`/"$link"
  fi
done

# 힙 메모리 용량을 확장
# 최소 1024m ~ 최대 2048
JAVA_OPTS="-Xms1024m -Xmx2048m" 

# Get standard environment variables
PRGDIR=`dirname "$PRG"`
```
> 참고 : default 힙 메모리는 64mb

```ps -ef | grep tomcat```
```.sh
vada     16122     1 99 07:39 ?        00:00:59 /usr/lib/jvm/default-java/bin/java -
Djava.util.logging.config.file=/var/lib/tomcat8/conf/logging.properties -
Djava.util.logging.manager=org.apache.juli.ClassLoaderLogManager -Xms1024m -Xmx2048m -XX:MaxPermSize=1024m -
Djava.endorsed.dirs=/usr/share/tomcat8/endorsed -classpath /usr/share/tomcat8/bin/bootstrap.jar:/usr/share/tomcat8/bin/tomcat-juli.jar -
Dcatalina.base=/var/lib/tomcat8 -Dcatalina.home=/usr/share/tomcat8 -Djava.io.tmpdir=/tmp/tomcat8-tomcat8-tmp 
org.apache.catalina.startup.Bootstrap start
```
