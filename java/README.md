#### 톰캣에서 사용중인 Heap 사이즈 확인
- ps -au|grep tomcat 명령어로 톰캣의 정보를 확인한다.
![ps 이미지](./images/ps-tomcat.png)
- jmap -heap [PID] 를 입력하여 JAVA의 힙 메모리 설정을 확인한다.
  (예. tomcat의 PID가 23631 경우 명령어는 jmap -heap 23631)
- MaxHeapSize 를 확인한다. 현재 최대로 설정되어 있는 힙사이즈는 250MB 이다.
![ps 이미지](./images/jmap.png)

#### JAVA Heap 사이즈 초기 설정 규칙

```
  Heap sizes 규칙
  Initial heap size of 1/64 of physical memory up to 1Gbyte
  Maximum heap size of 1/4 of physical memory up to 1Gbyte
  ```

&nbsp;&nbsp;나의 가상머신은 1GB 이므로 최소 Heap 사이즈는 16MB 이고 최대 Heap 사이즈는 250MB 이다.

#### JVM 모니터링 도구 VisualVM
NetBeans 플랫폼을 기반으로 개발된 VisualVM은 JVM을 실시간으로 모니터링하는 GUI 도구이다. 그 외에도 Heap Dump, Thread Dump도 할 수 있으며 여러개의 VM을 동시에 모니터링 및 프로파일링 할 수 있다.

##### 사용방법은

1. [VisualVM 다운로드](https://visualvm.github.io/download.html) 에서 다운로드를 한다.
2. 압축을 해제한 뒤 bin 폴더로 이동 한 뒤 visualvm.exe를 실행 한다.
3. Applications 탭에서 모니터링하고자 하는 목록을 클릭하면 오른쪽(모니터링, 스레드, 샘플러, 프로피일 탭)에서 확인할 수 있다.
![VisualVM](./images/visualvm.png)

##### 각 탭에 대한 설명
- Overview: 사용 옵션 및 자바 버전와 같은 시스템 정보 확인가능하다.
- Monitor: CPU, Memory, Classes, Threads 등을 볼 수 있다.
- Threads: 현재 Threads의 상태를 확인 가능하다.
- Sampler: CPU, Memory를 샘플링 할 수 있다. 즉, JVM의 일정 주기로 스레드 덤프를 통해서 성능을 측정하고 성능 측정에 영향을 거의 주지 않는다. 하지만 일정 주기로 스레드 덤프가 호출 되기 때문에 호출 횟수를 잃어버리는 경우가 생겨 정확하지 않다.
- Profiler: CPU, Memory 프로파일링을 할 수 있다. 즉, 어플리케이션 전체 혹은 몇몇 클래스의 성능을 측정 할 수 있고 알고리즘 최적화나 호출 횟수 측정하기에 용이하다.그리고 Profiler 에 의해 자동적으로 생성된 코드가 성능 측정에 어느정도 영향을 줄 수 있다.

#### DDD Start
- 도메인은 여러개의 하위 도메인을 가질 수 있다. 
- 도메인에 따라 용어의 의미가 결정된다.
- 여러 하위도메인을 하나의 다이어그램에 모델링하면 안된다. 이는 도메인을 이해하는데 방해가 된다.
- 도메인 모델 패턴
  |계층|설명|
  |----|----|
  |표현|사용자의 요청 처리 및 사용자에게 정보를 보여준다.|
  |응용|사용자가 요청한 기능을 실행한다. 업무로직을 직접 구현하지 않고 도메인 계층을 조합해서 기능을 실행한다.|
  |도메인|시스템이 제공할 도메인 규칙을 구현한다.|
  |인프라스트럭처|데이터베이스나 메시징 시스템과 같은 외부 시스템과의 연동을 처리한다.|

- 개념 모델: 순수하게 문제를 분석한 결과물이다. 이 모델은 데이터베이스, 트랜잭션 처리, 성능, 구현 기술등등과 같은 것을 고려하지 않기 때문에 실제 코드를 작성할 때는 개념 모델을 있는 그대로 사용할 수 없다. 그래서 개념 모델을 구현 가능한 형태의 모델로 전황하는 과정이 필요하다.
- 구현 모델: 구현하는 과정에서 개념 모델을 개발하면서 점차 발전해 나가는 모델의 형태(?)

- 도메인을 모델링할 때 기본이 되는 작업은 모델을 구성하는 핵심 구성요소, 규칙 기능을 찾는 것이다.
- 엔티티: 식별자를 갖는 객체이다. 식별자의 종류에는 특정 규칙에 따라 생성, UUID, 값을 직접 입력, 일련번호 사용(시퀀스나 DB의 자동증가 컬럼) 등이 있다.
- 밸류타입: 개념적을 완전한 하나를 표현 할 때 사용한다. 그리고 불변으로 구현하는 것을 권장한다.
- 도메인 모델에 set 메서드 넣지 않기.
- DIP(Dependency Inversion Principle, 의존 역전 원칙): 고수준의 모듈의 테스트를 하려면 저수준의 모듈에 의존해야 되는데 이렇게 될 경우 구현변경과 테스트의 어려움이 있다. 이를 해결하기위해 저수준의 모듈이 고수준의 모듈을 의존하도록 한다.
- 애그리거트(AGGREGATE): 애그리거트는 관련된 엔티티와 밸류 객체를 개념적으로 하나로 묶은 것이다. 도메인 모델에서 전체 구조를 이해하는데 도움이 된다.
- 리포지터리(REPOSITORY): 도메인 모델의 영속성을 처리한다.
- 도메인 서비스(DOMAIN SERVICE): 특정 엔티티에 속하지 않은 도메인 로직을 제공한다.
- 도메인 모델의 엔티티: 데이터와 함께 도메인 기능을 함께 제공한다. 예를 들어, 주문을 표현하는 엔티티는 주문과 관련된 데이터 뿐만 아니라 배송지 주소 변경을 위한 기능도 함께 제공한다. 그리고 두 개 이상의 데이터가 개념적으로 하나인 경우 밸류 타입을 이용해서 표현 할 수 있다.
- DB의 엔티티: 밸류 타입을 제대로 표현하기 힘들다. 단순하게 데이터를 담고 있다.

#### 톰캣 버전 숨기기
- 톰캣 설치 위치/conf/server.xml 파일을 연다.
- HOST 부분에 아래와 같이 입력한다.

```
<!-- 8080 기본포트에서 server 값을 공백으로 추가 -->
<Connector port="8080" protocol="HTTP/1.1"
               connectionTimeout="20000"
               redirectPort="8443" URIEncoding="UTF-8"
               server=" "/>

<!-- Host 안에 ErrorReportValve 를 추가한다. -->
<Host name="localhost"  appBase="webapps"
            unpackWARs="true" autoDeploy="true">

  <!-- SingleSignOn valve, share authentication between web applications
        Documentation at: /docs/config/valve.html -->
  <!--
  <Valve className="org.apache.catalina.authenticator.SingleSignOn" />
  -->

  <!-- Access log processes all example.
        Documentation at: /docs/config/valve.html
        Note: The pattern used is equivalent to using pattern="common" -->
  <Valve className="org.apache.catalina.valves.AccessLogValve" directory="logs"
          prefix="localhost_access_log." suffix=".txt"
          pattern="%h %l %u %t &quot;%r&quot; %s %b" />
  <Valve className="org.apache.catalina.valves.ErrorReportValve" showReport="false" showServerInfo="false"/>
</Host>
```

#### 톰캣 에러 페이지 커스텀
- 톰캣 설치 위치/conf/web.xml 파일을 연다.
- web.xml 파일의 맨 아래에 아래의 같이 에러 페이지를 정의 한다.
- html 파일은 톰캣 설치 위치/webapps/ROOT 안에 넣는다.

```
<web-app>
  ...
  <welcome-file-list>
      <welcome-file>index.html</welcome-file>
      <welcome-file>index.htm</welcome-file>
      <welcome-file>index.jsp</welcome-file>
  </welcome-file-list>

  <error-page>
      <error-code>404</error-code>
      <location>/error/404.html</location>
  </error-page>
  
  <error-page>
      <error-code>500</error-code>
      <location>/error/500.html</location>
  </error-page>
</web-app>
```

#### 아파치 버전 숨기는 방법
- httpd.conf 파일을 연다.
- ServerSignature 값을 Off 로 변경한다.
- ServerTokens 값을 Prod 로 변경한다.
  - Prod: 웹서버의 이름만 노출
  - Major: 웹서버의 이름과 Major 버전번호만 노출
  - Minor: 웹서버의 이름과 Minor 버전번호까지 노출
  - Min: 웹서버의 이름과 Minimum 버전까지 노출
  - OS: 웹서버의 이름과 버전, 운영체제까지 노출
  - Full: 최대한의 정보를 모두 노출

#### 아파치 커스텀 에러 페이지
- httpd.conf 파일을 연다.
- Alias /error/ "/var/www/error/" 이 부분을 찾는다.
- 해당 부분아래에 주석으로 된 ErrorDocument 가 존재한다.
- 주석을 해제하고 에러 페이지를 넣는다.
- HTML 파일은 /var/www/error 이라고 정의되어 있는데 해당 폴더에 넣는다.

```
Alias /error/ "/var/www/error/"

<IfModule mod_negotiation.c>
<IfModule mod_include.c>
    <Directory "/var/www/error">
        AllowOverride None
        Options IncludesNoExec
        AddOutputFilter Includes html
        AddHandler type-map var
        Order allow,deny
        Allow from all
        LanguagePriority en es de fr
        ForceLanguagePriority Prefer Fallback
    </Directory>

    ErrorDocument 400 /error/400.html
    ErrorDocument 401 /error/401.html
    ErrorDocument 403 /error/403.html
    ErrorDocument 404 /error/404.html
    ErrorDocument 405 /error/405.html
#    ErrorDocument 408 /error/HTTP_REQUEST_TIME_OUT.html.var
#    ErrorDocument 410 /error/HTTP_GONE.html.var
#    ErrorDocument 411 /error/HTTP_LENGTH_REQUIRED.html.var
#    ErrorDocument 412 /error/HTTP_PRECONDITION_FAILED.html.var
#    ErrorDocument 413 /error/HTTP_REQUEST_ENTITY_TOO_LARGE.html.var
#    ErrorDocument 414 /error/HTTP_REQUEST_URI_TOO_LARGE.html.var
#    ErrorDocument 415 /error/HTTP_UNSUPPORTED_MEDIA_TYPE.html.var
    ErrorDocument 500 /error/500.html
#    ErrorDocument 501 /error/HTTP_NOT_IMPLEMENTED.html.var
#    ErrorDocument 502 /error/HTTP_BAD_GATEWAY.html.var
#    ErrorDocument 503 /error/HTTP_SERVICE_UNAVAILABLE.html.var
#    ErrorDocument 506 /error/HTTP_VARIANT_ALSO_VARIES.html.var

</IfModule>
</IfModule>
```