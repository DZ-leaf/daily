CI Setting
==

### spring-boot -> github -> jenkins -> docker

1. spring-boot 프로젝트에 Dockerfile과 sh쉘 파일을 만든다.
        
        Dockerfile

        # spring-boot라서 jar파일을 실행할 jdk만 있으면 된다. 그렇기에 openjdk 1.8 버전을 베이스로 잡는다    
        FROM openjdk:8-jdk-slim
        
        # spring-boot를 운행하면서 tmp에 임시파일들이 생성되고 저장된다는데 일단 대부분의 Dockerfile설명부분에서 연결을 하길래 뭔가 있는가 싶어서 연결해놨지 정확하게는 모름 
        VOLUME /tmp

        # 이 maven 프로젝트를 ./mvnw clean package명령어를 실행시켜서 jar 파일을 만들것이고 그렇게 생성된 파일은 target폴더에 위와같은 이름으로 저장이 되기에 back.jar로 간단하게 바꿔서 docker에 넣어주려고 하는것이다
        ADD target/back-0.0.1-SNAPSHOT.jar back.jar

        # 매 docker 컨테이너를 실행시킬때마다 실행시킬 문구를 설정해준다.
        # jar 파일 실행을 적었기에 도커를 실행시키면 자동으로 서버가 켜진다.
        ENTRYPOINT ["java","-jar", "/back.jar"]

    <br>
        
        start.sh

        #!/bin/bash
        maintaner="dzleaf"
        app="back"
        container=`docker ps -a -q --no-trunc --filter name=^/$app`
        image=`docker images -q $maintaner/$app`
        script_path=$(dirname $(realpath $0))
        
        if [ -n "$container" ] || [ "$container" -eq "1" ] ; then
            docker rm -f "$container"
        fi
        
        if [ -n "$image" ] || [ "$image" -eq 1 ] ; then
            docker rmi "$image"
        fi
        
        "$script_path"/mvnw clean package
        
        docker build -t "$maintaner"/"$app" "$script_path"
        docker run -p 8888:8888 -d --name "$app" "$maintaner"/"$app"


    // 지금 도커파일 수정해야함 도커허브에 push해야하니깐

2. jenkins 설치

 
    서버 하나 준비하고 docker를 설치
    설치할 jenkins와 몇몇 설정을 위해 Dockerfile을 만든다

        Dockerfile

        # jenkins:lates버전은 실행했을때 최신버전이 아니여서 업데이트가 되지 않는다.
        # dockerhub에 jenkins 오피셜 말고 공식계정이 따로 올린 jenkins 이미지 파일이 있다. https://hub.docker.com/r/jenkins/jenkins 참고 (1.8이 없어서 11버전을 사용)
        FROM jenkins/jenkins:jdk11

        # sudo 명령어 하나하나 치기 귀찮으니 root로 변경
        USER root

        # 여기서 도커를 설치하는 이유는 나중에 이 dockerfile로 만들 컨테이너 명령어에 서버 docker와 볼륨을 잡을것이기 때문이다.
        # apt-get 하기엔 update도 해야하고 귀찮음
        RUN curl -fsSL https://get.docker.com -o get-docker.sh && \
            sh get-docker.sh && \
            usermod -aG docker jenkins
        
        # 유저를 다시 jankins로 돌려줌
        USER jenkins


    생성된 Dockerfile을 빌드하여 실행시킴

        docker build -t dzleaf/jenkins:0.1 .

        docker run -v /var/run/docker.sock:/var/run/docker.sock -d --privileged --name jenkins -p 11111:8080 jenkins
        docker logs jenkins


3. jenkins 설정

    jenkins 포트를 11111로 쓰기로 함
    11111포트로 들어가면 비밀번호를 요구하기에 logs 명령어로 비밀번호를 따옴

    기본 설치를 기다린 후 뜨는 메인에서 새 작업을 클릭
    item 이름 적고 freestyle project를 선택

    General 에서는 gitgub project 체크
    https://github.com/DZ-leaf/back.git 링크 등록

    소스코드 관리는 git으로 https://github.com/DZ-leaf/back.git같은 링크 등록

    빌드유발은 github hook

    빌드환경은 패스

    빌드는 execute shell

        chmod 777 start.sh ./mvnw
        ./start.sh

    저장

4. githook 설정

    git 프로젝트 생성한 계정으로 해야한다.

    project -> setting -> webhooks -> add

    서버아이피:11111/github-webhook/

    http://52.141.21.120:11111/github-webhook/ 적으면 됨


