#### 서비스 확인 방법
- CentOS 6
  - service [서비스이름] stop: 서비스 중지
  - service [서비스이름] start: 서비스 시작
  - service [서비스이름] restart: 서비스 재시작
  - service [서비스이름] status: 서비스 상태 확인
  - service [서비스이름] reload: 서비스의 설정파일을 리로드
- CentOS 7
  - systemctl stop [서비스이름]: 서비스 중지
  - systemctl start [서비스이름]: 서비스 시작
  - systemctl restart [서비스이름]: 서비스 재시작
  - systemctl status [서비스이름]: 서비스 상태 확인
  - systemctl reload [서비스이름]: 서비스의 설정파일을 리로드
- 톰캣 서버 버전 숨기기
  - server.xml 파일을 연다.
  - server 키의 값을 공백으로 아래와 같이 채운다.
  - 이렇게 하면 Server의 Apache-Coyote/1.1이 공백으로 출력된다.

  ```
  <Connector port="8080" protocol="HTTP/1.1"
               connectionTimeout="20000"
               redirectPort="8443" URIEncoding="UTF-8"
               server=" "/>
  ```

#### 톰캣 서버 Trace Method 차단하기
  - server.xml 파일을 연다.
  - allowTrace 키의 값을 false 로 아래와 같이 채운다.

  ```
  <Connector port="8080" protocol="HTTP/1.1"
               connectionTimeout="20000"
               redirectPort="8443" URIEncoding="UTF-8"
               server=" "
               allowTrace="false"/>
  ```

#### 톰캣 Allow Method 제한하기
  - server.xml 파일을 연다.
  - 위의 '톰캣 서버 Trace Method 차단하기' 를 적용한다.
  - web.xml 파일을 연다.
  - web.xml 파일 맨아래에 아래(</web-app> 바로위)와 같이 제한할 allow method를 작성한다.
  - 서버를 재시작 한다.

  ```
  <security-constraint>
		<display-name>Forbidden</display-name>
		<web-resource-collection>
			<web-resource-name>restricted methods</web-resource-name>
			<url-pattern>/*</url-pattern>
			<http-method>PUT</http-method>
			<http-method>HEAD</http-method>
			<http-method>HEAD</http-method>
			<http-method>DELETE</http-method>
			<http-method>TRACE</http-method>
			<http-method>COPY</http-method>
			<http-method>MOVE</http-method>
		</web-resource-collection>
		<auth-constraint />
	</security-constraint>
    <security-constraint>
        <web-resource-collection>
            <web-resource-name>Protected Context</web-resource-name>
            <url-pattern>/servlet/org.apache.catalina.servlets.DefaultServlet/*</url-pattern>
        </web-resource-collection>
        <user-data-constraint>
            <transport-guarantee>CONFIDENTIAL</transport-guarantee>
        </user-data-constraint>
    </security-constraint>
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

#### Spring Allow Method 제한하기(위의 톰캣과 같이 진행해야 한다.)
  - spring 프로젝트의 web.xml 파일을 연다.
  - `<web-app>` 안에 아래와 같이 입력한다.
  - `<servlet>` 안에 dispatchOptionsRequest 옵션을 true로 추가한다.

  ```
  <!-- 서블릿 파일 설정 -->
	<servlet>
		<servlet-name>HelloWorld</servlet-name>
		<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
		<init-param>
			<param-name>contextConfigLocation</param-name>
			<param-value>/WEB-INF/hw-servlet.xml </param-value>
		</init-param>
		<init-param>
			<param-name>dispatchOptionsRequest</param-name>
			<param-value>true</param-value>
		</init-param>
		<load-on-startup>1</load-on-startup>
	</servlet>

  <security-constraint>
		<display-name>Forbidden</display-name>
		<web-resource-collection>
			<web-resource-name>restricted methods</web-resource-name>
			<url-pattern>/*</url-pattern>
			<http-method>PUT</http-method>
			<http-method>HEAD</http-method>
			<http-method>HEAD</http-method>
			<http-method>DELETE</http-method>
			<http-method>TRACE</http-method>
			<http-method>COPY</http-method>
			<http-method>MOVE</http-method>
		</web-resource-collection>
		<auth-constraint>
			<role-name></role-name>
		</auth-constraint>
	</security-constraint>
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

#### 아파치 Method 제한하기
- httpd.conf 파일을 연다.
- Location 부분을 추가하여 제한할 메소드를 정의한다.
- LimitExcept는 허용가능한 메소드이고 Limit는 허용하지 않는 메소드 이다.
- TraceEnable Off 도 추가한다.(trace method는 Limit 가 아닌 TraceEnable로 Off 하면 된다.)

```
<Location />
	<LimitExcept GET POST OPTION PUT>
		Order allow,deny
    		Allow from all
	</LimitExcept>
#    <Limit TRACE>
#        Order allow,deny
#            Allow from all
#    </Limit>
</Location>

TraceEnable off
```