## 서버 구성하기

1. 바탕화면에 새폴더로 프로젝트 폴더 만들기

2. node.js 설치

   - nodejs.org 홈피에 접속
   - 설치파일 node-v20.15.0-x64.msi 를 새폴더에 다운로드
   - node-v20.15.0-x64.msi 실행하여 program Files > nodejs 에 설치, 모두 next 하고 install 버튼 클릭

3. vs code 에서 서버 시작하기

   - 프로젝트 폴더에서 git bash하고 node . 로 vscode시작
   - 루트에 index.js 만들기
   - console.log("hello world"); 코딩하고 저장 ctl + s
   - 서버터미널 시작하기 ctl + shift + `
   - node 설치 확인
     - index.js 실행 : >node index.js 실행
     - hello world가 보이면 node 설치 잘된거임
     - tip : 오류메세지를 복사해서 구글에 올리면 대부분 나옴

4. npm 에서 필요, 편리한 툴 가져다 쓰기 (npm홈피에서 모듈, 사용법등 참조)

   - npm으로 설치한 모듈 기록 만들기

     - 터미널에서 npm init 실행하고 조회되는 옵션 엔터로 지나가기 (패키지는 뭐고, entry point는 index.js이고.. 등등 )
     - npm init 실행하면 package.json이 root 에 생성되고
     - npm으로 설치되는 모든 모듈이 파일에 기록된다
     - 향후 이 프로젝트를 외부 서버에 Deploy 할때 필요모듈 설치의 기준이 됨

   - 모듈 설치 방법 : npm install [라이브러리 이름] (-g 를 붙이면 내컴퓨터 모든 디렉토리에 적용)

     - npm 연습 : npm install figlet : 큰 글자 만들기
     - package.json 의 dependencies 에 모듈 설치했다는 기록 생김, package-lock.json 에는 더욱 자세한 정보 기록
     - root에 node_modules 디렉토리가 생성되고 설치된 모듈 디렉토리가 보이고 그 안에 엄청 많은 프로그램 보임.

   - 모듈 삭제 방법 : npm uninstall [라이브러리 이름]

5. express 포함 모듈 설치

   - express : Node.js를 위한 빠르고 개방적인 간결한 웹 프레임워크
   - bcrypt : salting과 키 스트레칭을 구현한 해시 함수 중 대표적인 함수
   - body-parser : Express 애플리케이션에서 요청의 본문을 해석하고 파싱하는 미들웨어. 클라이언트로부터 전송된 JSON, URL-encoded 및 기타 형식의 데이터를 해석하여 JavaScript 객체로 변환
   - cors : SOP는 2011년 RFC 6454에서 등장한 보안 정책으로 "같은 출처에서만 리소스를 공유할 수 있다"라는 규칙을 가진 정책을 해결하는 것
   - dotenv : .env라는 외부 파일에 중요한 정보를 환경변수로 저장하여 정보를 관리. .gitignore에서 .env 제외
   - express-session : Create a session middleware with the given options.세션 데이터를 서버에 저장하고 클라이언트에 고유한 세션 식별자를 부여하여 상태를 유지
   - multer : 파일 업로드를 위해 사용되는 multipart/form-data 를 다루기 위한 node.js 의 미들웨어
   - mysql : SQL이라고 하는 구조화된 쿼리 언어를 사용하여 데이터를 정의, 조작, 제어, 쿼리하는 관계형 데이터베이스
   - path : 프로그램을 찾는 기본 경로를 담당
   - session-file-store : 세션이 데이터를 저장하는 곳. 대표적으로 Memory Store, File Store, Mongo Store.

6. index.js에 API서버 로직 구현

7. index.js 기동

   - terminal 에서 node index.js 로 기동
   - ctl + C 로 해제,
   - API서버 수정시마다 기동/해제 해줘야 로직 반영됨
   - 기동/해제 자동화 : Nodemon 모듈 설치

8. maria db 서비스 올리기 (cloudtype이용)

   - mariadb 서비스 생성

     - service name = mariasvc (host name으로 사용됨. 이름에 언더바 사용은 안되고 '-'사용은 됨)
     - root pwd = pwd는 .env 에 설정 (지정안하면 자동생성이됨, 자동생성이면 내가 모르잖아?)
     - 배포하기 클릭후 서비스로 이동하여 실행로그 확인 (API서버에 내가 코딩한 대기포트, DB연결 console.log 확인)

   - 실행로그에 서비스가 생성완료 조회되면 터미널페이지에서 db, user 생성하기
     - root 로 들어가기 : pwd는 service 생성시 지정했던 패스워드를 입력
     - db 생성 : CREATE DATABASE picdb CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci; 참조-CREATE DATABASE [DB_NAME] CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
     - uesr 생성 : CREATE USER yang@mariasvc IDENTIFIED BY '1234'; 참조-CREATE USER [USER_NAME]@[HOST_NAME] IDENTIFIED BY '[PASSWORD]';
     - user 권한 부여 : GRANT ALL PRIVILEGES ON picdb.별 TO yang@mariasvc; 참조-GRANT ALL PRIVILEGES ON [DB_NAME].별 TO [USER_NAME]@[HOST_NAME];

9. 테이블 만들기

   - DB tool : heidiSQL 사용
   - seeeion 만들기
     - 호스트명 / IP : svc.sel5.cloudtype.app, cloudtype > DB서비스 > 연결 : 외부연결(svc.sel5.cloudtype.app:30738) 의 :앞자리가 호스트명이 됨
     - 사용자 : root
     - 암호 : root 패스워드
     - 포트 : 30738, 외부연결(svc.sel5.cloudtype.app:30738) 의 :뒷자리 30738 가 포트번호가 됨, DB 서비스 생성시마다 포트번호 상이함
   - 원하는 테이블 쿼리로 생성 ex. CREATE TABLE [테이블명] (컬럼들...)

10. 서버 서비스 올리기 (cloudtype이용)

- Node js 서비스 생성
  - git 저장소 설정 : yang, branch는 메인의 이름 설정
  - Node js 버전 선택 : v20, 서버 터미널에서 node -v 하면 v 20. 2....식으로 조회되는 버전 선택
  - 환경변수 세팅 : .env, 탐색기에서 API서버 프로젝트에 있는 .env파일을 클릭으로 끌어서 옮겨줌, 변경필요시 세팅화면에 직접 입력
  - port : 5000, API서버 포트
  - Install command : npm ci, package-lock.json을 인스톨함, 공란시 package.json이 인스톨됨
  - Start command : index.js, API서버프로그램을 지정해야 함
  - 배포하기 클릭후 서비스로 이동하여 실행로그 확인

11. 프론트 올리기

- 로컬에서 리모트 서버와 연결 확인
