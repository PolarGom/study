### MySQL Troubleshooting
- API 혹은 어플레키에션이 운영 중일때 쿼리 로그를 확인하는 방법?
  - Log 에는 General log(모든 Query log 수집), Slow log(SQL 질의 요청을 했는데 응답이 오래 걸리는 log 를 수집), Error log 가 있는데 그 중에서 General log 수집 방법을 사용 한다.
  - 설정 방법
    1. MySQL 에 접속을 한다.
    2. 아래의 쿼리를 실행 후 general_log 값이 ON 이 되어 있는지 확인한다.

    ```
    query> SHOW variables LIKE 'general%';
    ```

    3. general_log 가 OFF 라면 아래의 쿼리를 실행 후 다시 2번 쿼리를 실행하여 general_log 값이 ON 이 되었는지 확인한다.

    ```
    query> SET GLOBAL general_log = 'on';
    ```

    4. 정상적으로 실행되었다면 이제 로그를 테이블로 보기 위해 LOG_OUTPUT 을 table 로 설정한다.

    ```
    query> SET GLOBAL LOG_OUTPUT = 'table';
    ```

    5. 설정 후 정상적으로 LOG_OUTPUT 값이 table 로 되어있는지 확인한다.

    ```
    query> SHOW variables LIKE 'log_output%';
    ```

    6. 설정이 완료되면 아래의 쿼리를 실행하면 MySQL 서버에 쌓이는 로그를 확인 할 수 있다.

    ```
    query> SELECT * FROM mysql.general_log;
    ```

    7. 확인이 끝났으면 general_log 값을 off 로 변경한다. off 로 변경하는 이유는 많은 쿼리가 MySQL 서버로 들어오게 되면 성능상의 이슈가 생길 수 있기 때문이다.

    ```
    query> SET GLOBAL general_log = 'off';
    ```


