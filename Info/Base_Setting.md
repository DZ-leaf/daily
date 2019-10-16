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
        

3. spring - postgreSQL 연결

- pom.xml에 아래 코드 추가
        
        <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-starter-jdbc</artifactId>
        </dependency>
        <dependency>
                <groupId>org.postgresql</groupId>
                <artifactId>postgresql</artifactId>
        </dependency>

- application.properties에 아래 코드 추가

     
        # application.properties
        spring.datasource.hikari.maximum-pool-size=4
        spring.datasource.driverClassName=org.postgresql.Driver
        spring.datasource.url=jdbc:postgresql://jh-rds.c6i36spuwi8p.ap-northeast-2.rds.amazonaws.com:5432/leaf

        spring.datasource.username=postgres
        spring.datasource.password=rootroot
   
- java파일 생성

import java.sql.Connection;
import java.sql.Statement;

import javax.sql.DataSource;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.ApplicationArguments;
import org.springframework.boot.ApplicationRunner;
import org.springframework.jdbc.core.JdbcTemplate;
import org.springframework.stereotype.Component;

@Component
public class Postgre implements ApplicationRunner {

    @Autowired
    DataSource dataSource;

    @Autowired
    JdbcTemplate jdbcTemplate;

    @Override
    public void run(ApplicationArguments args) throws Exception {
        System.out.println
        try(Connection connection = dataSource.getConnection()){
            System.out.println
            System.out.println(connection);
            String URL = connection.getMetaData().getURL();
            System.out.println(URL);
            String User = connection.getMetaData().getUserName();
            System.out.println(User);

            // Statement statement = connection.createStatement();
            // String sql = "CREATE TABLE ACCOUNT(" +
            //         "ID INTEGER NOT NULL," +
            //         "NAME VARCHAR(255)," +
            //         "PRIMARY KEY(ID))";
            // statement.executeUpdate(sql);
            // 주석은 statement사용법
        }

        jdbcTemplate.execute("INSERT INTO ACCOUNT VALUES(2, 'asd')");
        // 위 코드도 간단한 예제
    }
}

                


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