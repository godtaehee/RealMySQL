# MySQL

MySQL 서버가 설치된 디렉터리는 /usr/local/mysql이며 하위 디렉터리 정보는 다음과 같다. 적어도 이 디렉터리만큼은 절대 삭제하면 안된다.

- bin: MySQL 서버와 클라이언트 프로그램, 유틸리티를 위한 디렉터리
- data: 로그 파일과 데이터 파일들이 저장되는 디렉터리
- include: C/C++ 헤더 파일들이 저장된 디렉터리
- lib: fkdlqmfjfl vkdlfemfdl wjwkdehls elfprxjfl
- share: 다양한 지원 파일들이 저장돼 있으며, 에러 메시지나 샘플 설정 파일(my.cnf)이 있는 디렉터리

![Screen Shot 2021-10-11 at 5 19 29 PM](https://user-images.githubusercontent.com/44861205/136756586-feaab78c-5326-46ea-af66-f5fbb9107451.png)


macOS에 설치된 MySQL 서버의 설정 파일(my.cnf) 등록 및 시작과 종료는 위의 사진처럼 모두 `System Preferences`의 최하단에 있는 MySQL을 클릭하면 실행되는 MySQL 관리 프로그램에서 수행할 수 있다.

이러한 GUI 관리프로그램이 아닌 Terminal에서 MySQL 서버를 실행하거나 종료하고 싶다면 다음과 같은 명령어로 가능하다.

```shell
## MySQL 서버 시작
sudo /usr/local/mysql/support-files/mysql.server start

## MySQL 서버 종료
sudo /usr/local/mysql/support-files/mysql.server stop
```

![Screen Shot 2021-10-11 at 5 26 27 PM](https://user-images.githubusercontent.com/44861205/136757651-4a5299b0-e2aa-4096-a33d-633b964ce47a.png)

MySQL이 서버를 실행하는 데 필요한 파일은 초기 데이터파일(시스템 테이블이 저장되는 데이터 파일)과 트랜잭션 로그(리두 로그) 파일이있다.

### MySQL 서버 연결 테스트

MySQL 서버에 접속하는 방법은 MySQL 서버프로그램(mysqld)과 함께 설치된 MySQL 기본 클라이언트 프로그램인 mysql을 실행하면 된다. 여러 가지 형태의 명령행 인자를 넣어 접속을 시도할수 있다.

```shell
# 1
mysql -uroot -p --host=localhost --socket=/tmp/mysql.sock
# 2
mysql -uroot -p --host=127.0.0.1 --port=3306
@ 3
mysql -uroot -p
```

첫 번째 예제는 MySQL 소켓 파일을 이용해 접속하는 예제다.

두 번째 예제는 TCP/IP를 통해 127.0.0.1(로컬 호스트)로 접속하는 예제인데, 이 경우에는 포트를 명시하는 것이 일반적이다.

**로컬 서버에 설치된 MySQL이 아니라 원격 호스트에 있는 MySQL 서버에 접속할 때는 반드시 두 번째 방법을 사용해야 한다.**

MySQL서버에 접속할 때는 호스트를 `localhost`로 명시하는 것과 `127.0.0.1`로 명시하는 것이 각각 의미가 다르다.

`--host=localhost` 옵션을 사용하면 MySQL 클라이언트 프로그램은 항상 소켓 파일을 통해 MySQL 섭버에 좁속하게 되는데, 이는 'Unix domain socket'을 이용하는 방식으로 TCP/IP를 통한 통신이 아니라 유닉스의 프로세스 간 통신(IPC: Inter Process Communication)의 일종이다. 하지만 `127.0.0.1`을 사용하는 경우에는 자기 서버를 가리키는 루프백(loopback) IP이기는 하지만TCP/IP 통신 방식을 사용하는 것이다.

세 번째 방식은 별도로 호스트 주소와 포트를 명시하지 않는다. 이 경우에는 기본값으로 호스트는 `localhost`가 되며 소켓 파일을 사용하게 되는데, 소켓 파일의 위치는 MySQL 서버의 설정 파일에서 읽어서 사용한다. MySQL 서버가 기동될 때 만들어지는 유닉스 소켓 파일은 MySQL 서버를 재시작하지 않으면 다시 만들어 낼 수 없기 때문에 실수로 삭제하지 않도록 주의한다. 유닉스나 리눅스에서 mysql 클라이언트 프로그램을 실행하는 경우에는 mysql 프로그램의 경로를 `PATH` 환경변수에 등록해 둔다.
