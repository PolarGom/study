#### Jitsi Docker 실행 방법
1. https://github.com/jitsi/docker-jitsi-meet/releases/tag/stable-6433 에 접속하여 하단의 Source code(zip)을 다운받는다.
2. 다운받은 파일을 압축 해제 후 해당 폴더로 이동 한다.
3. .env.example 파일을 .env 파일의 이름으로 복사한다.(명령어 하단 참조)

```
cp env.example .env
```

4. ./gen-passwords.sh 쉘 파일을 실행합니다.(명령어 하단 참조)

```
./gen-passwords.sh
```

5. CONFIG 폴더를 생성한다.(명령어 하단 참조)

```
# 리눅스 일 경우
mkdir -p ~/.jitsi-meet-cfg/{web/crontabs,web/letsencrypt,transcripts,prosody/config,prosody/prosody-plugins-custom,jicofo,jvb,jigasi,jibri}

# 윈도우 일 경우
echo web/crontabs,web/letsencrypt,transcripts,prosody/config,prosody/prosody-plugins-custom,jicofo,jvb,jigasi,jibri | % { mkdir "~/.jitsi-meet-cfg/$_" }
```

6. .env 설정파일에서 알맞게 설정을 변경한다.

```
# Exposed HTTP port
HTTP_PORT=8000

# Exposed HTTPS port
HTTPS_PORT=9443

# System time zone
TZ=Asia/Seoul

# Public URL for the web service (required)
# 도메인이 없다면 https://www.freenom.com 에서 무료 도메인 발급
PUBLIC_URL=https://eda-dev.tk:9443

# IP address of the Docker host
DOCKER_HOST_ADDRESS=192.168.0.83

# Domain for which to generate the certificate
LETSENCRYPT_DOMAIN=eda-dev.tk:9443

# E-Mail for receiving important account notifications (mandatory)
LETSENCRYPT_EMAIL=phkaa@edacom.co.kr

# Enable Let's Encrypt certificate generation
ENABLE_LETSENCRYPT=0

# Media port for the Jitsi Videobridge
JVB_PORT=10000
```

7. docker-compose 명령어를 실행한다.(명령어 하단 참조)

```
docker-compose up -d 
```

8. 정상적으로 실행이 되었으면 https://localhost:8443 로 접속하면 된다.

#### Jitsi Components
- Jitsi Meet
- Jitsi Videobridge(JVB)
- Jitsi Conference Focus(jicofo)
- Jitsi Gateway to SIP(jigasi)
- Jitsi Broadcasting Infrastructure(jibri)
- Prosody