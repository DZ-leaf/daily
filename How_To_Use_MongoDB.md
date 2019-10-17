<p><h1 align="center" style="font-family: Georgia, 'Times New Roman', serif">MongoDB</h1></p>

1. MongoDB
   * Document-Oriented(문서 지향적) Cross-Platform NoSQL DataBase
   * Open Source이며 엔진은 C++로 작성됨
   * RDBMS(관계형 데이터베이스)의 한계를 극복하기 위한 새로운 형태의 DataBase
   * 뛰어난 확장성과 성능을 자랑함  
    <br>
2. Document
   * RDBMS의 record와 유사한 개념
   * JSON Object 형태의 한 개 이상의 key-value의 쌍으로 이루어진 데이터 구조로 구성
   * value에는 다른 document, array, document array가 포함될 수 있음  
       <br>
       <center><img src="./images/mongodb/mongodb_document.PNG"></center>  
       <br>  
   * Document의 형태
      * 각 Document는 **_id**라는 고유한 값을 가짐
      * 이 고유한 값은 시간/ 머신 ID/ 프로세스 ID/ 순차번호 순으로 구성됨
      * 값의 고유성 보장  
        *ex)* 
        <br>
        <center><img src="./images/mongodb/document_form.PNG"></center>  
        <br>  
3. _id
   * 모든 MongoDB의 collection은 기본적으로 _id field에 index가 존재함
   * 만약 collection을 만들 때 _id field를 따로 지정하지 않으면 mongod 드라이버가 자동으로 _id field 값을 ObjectId로 설정해 줌
   * _id는 12bytes의 hexadecimal(16진수) 값으로, 첫 4byte는 현재 timestamp, 다음 3byte는 machine id, 다음 2byte는 MongoDB Server의 Process id, 마지막 3byte는 순차번호

4. Collection 
    * RDBMS의 table과 유사한 개념으로 Document들의 집합으로 구성

5. Database
    * Collection들의 물리적인 컨테이너
    * 0개 이상의 Collection들의 집합으로 구성되며 Collection은 0개 이상의 Document로 구성되고 Document는 1개 이상의 field로 구성

6. RDBMS(MySQL) vs MongoDB
    * Terms  
        | **RDB(MySQL)** | **MongoDB** |
        |:--------:|:--------:|
        | Database | Database |
        | Table | Collection |
        | Tuple / Row | Document / BSON(Binary JSON) document |
        | Column | Field |
        | Table Join | Embedded Documents & Linking |
        | Primary Key | Primary key (_id) |

    * Database Server
        | **RDB(MySQL)** | **MongoDB** |
        |:--------:|:--------:|
        | mysqld | mongod |

    * Database Client
        | **RDB(MySQL)** | **MongoDB** |
        |:--------:|:--------:|
        | mysql | mongo |

    * SQL
        || **RDB(MySQL)** | **MongoDB** |
        |:--------:|:--------:|:--------:|
        | Insert | insert into users("name", "city") values ("lee", "seoul") | db.users.insert({ name: "lee", city: "seoul" }) |
        | Select | select * from users where name="lee" | db.users.find({ name: "lee" }) |
        | Update | update users set city="busan" where name="lee" | db.users.update({ name: "lee" }, {$set:{ city: "busan" }}) |
        | Delete | delete from users where name="lee" | db.users.remove({ name: "lee" }) |

7. 사용법
    * 단순히 mongoDB는 mongod(Server)를 실행하면 인증 과정없이 바로 사용이 가능
    * 인증 및 인가 측면에서 굉장히 위험하므로 인증 및 인가에 대한 Rule이 필요
    * 사용자를 추가한 후 사용자 외에는 접근이 불가능하도록 하는 방법이 있음
    * Database
    * Database User
        | **Name** | **Description** |
        |:--------:|:--------:|
        | db.auth() | Database에 사용자 인증 |
        | db.createUser() | Creates a new user |
        | db.updateUser() | Updates user data |
        | db.changeUserPassword() | 사용자 Password 변경 |
        | db.dropAllUsers() | Database에 관련된 모든 사용자 삭제 |
        | db.dropUser() | 한 사용자 삭제 |
        | db.grantRolesToUser() | Role과 권한을 사용자에게 부여 |
        | db.revokeRolesFromUser() | 사용자에게 부여한 Role 삭제 |
        | db.getUser() | 지정한 사용자의 정보 반환 |
        | db.getUsers() | Database에 관련된 모든 사용자의 정보 반환 |


8. 특징
    * Schema-less => RDBMS처럼 고정 Schema가 존재하지 않다는 뜻
    * 같은 Collection 내에 있더라도 document level의 다른 Schema를 가질 수 있다는 의미
    * RDBMS와 같은 JOIN 연산이 없어 Table JOIN은 효과적이지 않지만(불가능하지는 않음) CRUD Query는 고속으로 동작함
    * MongoDB는 Schema를 디자인할 때 하나의 document에 최대한 많은 데이터를 포함시킴
    * Scalability(규모 가변성, 확장성)이 우수하며 Sharding(여러 개의 Database에 데이터를 분할하는 기술) 클러스터 구축도 가능함

9.  MongoDB install in Centos7
    * 사전 작업
        * epel 저장소 추가
    * 확인
        * service mongod status => mongod: unrecognized service
        * mongo --version => -bash: mongo: command not found
    * 설치
        * 서버(mongodb-server)만 설치해도 되지만, 클라이언트(mongodb)도 함께 설치
        * yum install mongodb mongodb-server
    * 서비스 시작
        * service mongod status -> mongod is stopped
        * service mongod start 또는 systemctl start mongod
    * 설치 확인
        * mongo --version => MongoDB shell version: 2.6.12
        * netstat -ntlp | grep mongod => tcp        0      0 127.0.0.1:27017             0.0.0.0:*                   LISTEN      29813/mongod