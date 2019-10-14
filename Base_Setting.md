<p><h1 align="center" style="font-family: Georgia, 'Times New Roman', serif">Initial Setting</h1></p>

1. expo 세팅

            npm install -g expo-cli
            expo init [폴더명]
            cd [폴더명]
            npm start

2. spring 세팅

        extensions 설치 목록
        - Java Extension Pack(Micosoft)
        - Spring Boot Extension Pack(Pivotal)

        [파일] - 기본설정 > 설정 > setting.json 편집 > "java.home":"경로" 추가

        Ctrl + Shift + P > maven project > Java > GroupID > ArtifactID > 최신버전 > Dev tool , spring web 선택
        
        .\mvnw spring-boot:run
        




<!--
1. centos 초기 세팅
    - expo 설치    

            yum update -y
            curl -sL https://rpm.nodesource.com/setup_10.x | bash -
            sudo yum clean all && sudo yum makecache fast
            sudo yum install -y gcc-c++ make
            sudo yum install -y nodejs
            npm install -g expo-cli
            expo init [폴더명]
            cd [폴더명]
            npm start
-->