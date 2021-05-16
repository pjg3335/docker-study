## docker-composs란?

여러개의 docker를 실행시켜주는 툴.   
예를들어 데이터베이스의 더미데이터를 만들어 주는 툴인 [generatedata](https://generatedata.com/)를 local에서 사용하려면 *PHP서버*와 *MYSQL서버* 2가지가 필요한데 이를 따로 실행하는게 아니라 yaml형식의 파일의 정보를 기준으로 한번에 실행함  
(docker 설치할때 같이 설치되기 때문에 따로 설치 필요없음)

<br/>

## 예제
아무 디렉토리에 다음의 파일을 만든다.  

**docker-compose.yml**
```yml
version: '3'
services:
  generatedata:
    # Replace `image` with `build: ..` in order to build the image locally
    # build: ..
    image: ghcr.io/mvisonneau/generatedata:latest
    environment:
      GD_DB_HOSTNAME: mysql
      GD_DB_NAME: generatedata
      GD_DB_USERNAME: root
    ports:
      - 9999:8080 #호스트:컨테이너
    links:
      - mysql

  mysql:
    image: docker.io/library/mariadb:10.4
    environment:
      MYSQL_ALLOW_EMPTY_PASSWORD: 'true'
      MYSQL_DATABASE: generatedata
```

<br/>

같은 디렉토리에 terminal을 열고 다음의 명령어 실행
```bash
# 실행
docker-compose up -d # -d: background 실행
# 동작중인 컨테이너 상태 확인
docker-compose ps
```

<br/>
 
조금 기다린 후 브라우저에서 `http://localhost:9999/`로 접속

<br/>

다음의 명령어로 종료 
```
docker-compose down
```

[관련링크](https://github.com/mvisonneau/docker-generatedata)