### 도커 명령어
|명령어|설명|비고|
|---|---|---|
|docker run [옵션] [이미지 이름]:[태그]|이미지를 컨테이너로 생성 후 실행||
|docker stop [컨테이너 ID]:[태그]|컨테이너 중지||
|docker start [컨테이너 ID]:[태그]|컨테이너 시작||
|docker attach [컨테이너 ID]:[태그]|실행되고 있는 컨테이너에 표준 입력(stdin)과 표준 출력(stdout)을 연결||
|docker exec|컨테이너에 접속 하여 특정 명령을 실행(/bin/bash), 접속 종료는 exit||
|docker images|도커 이미지 조회||
|docker search [이미지 이름]|도커 이미지 검색하기||
|docker pull [이미지 이름]:[태그]|도커 이미지 생성하기||
|docker rmi [이미지 ID]|도커 이미지 삭제하기||
|docker ps -a|도커 모든 컨테이너 목록 보기|-a 가 빠지면 현재 실행되고 있는 목록만 조회|
|docker container logs -t [컨테이너 ID]|로그 확인||
|docker rm [컨테이너 이름]|컨테이너 삭제||
|docker --help|명령어 확인||
|docker rename [기존 이름] [변경 할 이름]|컨테이너 이름 변경||

### 도커 옵션
|옵션|설명|비고|
|---|---|---|
|-d|백그라운드 모드||
|-p [호스트포트]:[컨테이너포트]|호스트와 컨테이너의 포트 연결(포트포워딩)||
|-v [호스트 폴더]:[컨테이너 폴더]|호스트와 컨테이너의 폴더 마운트(연결)||
|-e|컨테이너 내에서 사용 할 환경 변수 설정|Dockerfile의 ENV 설정도 덮어씌워짐|
|--name|컨테이너 이름 설정||
|-it|터미널 입력|쉘이나 CLI 도구를 사용할 때 유용함|
|-w|WORKDIR 설정을 덮어씌움||
|--entrypoint|Dockerfile의 ENTRYPOINT 설정을 덮어씌움||
|--rm|컨테이너 일회성 실행|컨테이너가 종료될 때 컨테이너와 관련된 리소스까지 제거됨|
|--link [컨테이너명]:[별칭]|컨테이너 연결||
|--restart|docker desktop을 실행 시킬 때마다 컨테이너를 자동으로 재시작 실행 여부|no: 컨테이너를 재시작 시키지 않음(default), on-failure:[max-retries]: 컨테이너가 정상적으로 종료되지 않은 경우 재시작 시킴. max-retires와 함께 옵션을 주면 최대 시도 횟수 설정, always: 컨테이너를 항상 재시작 시킴, unless-stopped: 컨테이너를 Stop 시키기 전까지 항상 재시작 시킴.|
|--log-opt max-size=100m --log-opt max-file=5|최대 100메가 크기의 5개의 로그 파일이 로그 로테이트 됨||

### 참고
- [도커 네트워크 요약](https://jonnung.dev/docker/2020/02/16/docker_network/)