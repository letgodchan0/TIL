# ⛵️ Docker 기초

>컨테이너 기반의 오픈소스 가상화 플랫폼으로 프로그램의 배포 및 관리를 단순하게 해준다.  간단하게 말하면 컨테이너라는 격리된 환경에서 다양한 프로그램을 실행할 수 있게 해주는 가상화 플랫폼이다. 복잡한 서버 환경을 코드로 쉽게 관리할 수 있고 무준단 배포환경을 구성화면서 추후 CI/CD를 설계하는데 있어 필수적인 플랫폼이다.

<br>

## ⚒ 설치

```bash
$ sudo apt-get update
$ sudo apt-get install docker.io
$ sudo ln -sf /usr/bin/docker.io /usr/local/bin/docker
```

<br>

## 🧲 빌드

#### - spring boot jar 파일

```cmd
docker build --build-arg JAR_FILE=build/libs/*.jar -t myImages/back:latest .
```

<br>

#### - react 파일

```cmd
docker build -t myImages/front:latest .
```

<br>

## 📰 Image

> 예를들어 sprinboot 배포를 위해 빌드한 파일을 이미지화 하여 docker hub에 저장하거나 로컬에 저장을 하고 이 이미지를 가지고 container를 실행할 수 있다.

<br>

#### - 이미지 목록 보기

```bash
$ sudo docker images
```

<br>

#### - 이미지 받기

```bash
$ sudo docker pull [이미지 이름]:[버전]
$ sudo docker pull myHub/back:latest
```

<br>

#### - 이미지 검색

```bash
$ sudo docker search [image 이름]
$ sudo docker search myHub/back:latest
```

<br>

#### - 이미지 삭제

```bash
$ sudo docker rmi [이미지 id]
$ sudo docker rmi myHub/back:latest

$ sudo docker rmi -f [이미지 id]
```

- -f 옵션을 주면 컨테이너도 강제 삭제가 가능하다!!

<br>



## 🧊 Container

> Nginx, Database 등 다양한 프로그램을 컨테이너라는 격리된 환경을 통해 실행시킬 수 있다!

<br>

#### - 컨테이너 목록

```bash
$ sudo docker ps
$ sudo docker ps -a
```

- -a 옵션을 주면 모든 컨테이너 목록을 출력할 수 있다. 

<br>

#### - 컨테이너 실행

```bash
$ sudo docker run [options] image[:TAG|@DIGEST] [COMMAND] [ARG...]
$ sudo docker container run [options] image[:TAG|@DIGEST] [COMMAND] [ARG...]

$ sudo docker container run --name front -d -p 3000:80 myDocker/front:latest
```

- -d : 백그라운드 모드로 실행
- -p : 호스트와 컨테이너의 포트를 연결하는 포워딩 옵션
- -v : 호스트와 컨테이너의 디렉토리를 연결하는 마운트 옵션
- -e : 컨테이너 내에서 사용할 환경변수 설정
- --name : 컨테이너 이름 설정
- --it : 터미널 입력을 위한 옵션
- --rm : 프로세스 종료시 컨테이너 자동 제거

<br>

#### - 컨테이너 시작

```bash
$ sudo docker start [컨테이너 id 또는 name]
```



<br>

#### - 컨테이너 재시작

```bash
$ sudo docker restart [컨테이너 id 또는 name]
```



<br>

#### - 컨테이너 정지

```bash
$ sudo docker stop [컨테이너 id 또는 name]
```



<br>

#### - 컨테이너 삭제

```bash
$ sudo docker rm [컨테이너 id 또는 name]
```



<br>

#### - 모든 컨테이너 삭제

```bash
$ sudo docker rm `docker ps -a -q`
```

