### 1. HTTP 리퀘스트 메시지 작성
#### 1. URL 입력
- URL : 자원이 어디있는지 알려주는 규약
- http: 뿐만아니라 file:, mailto: 등이 있음
- 브라우저는 웹 서버에 엑세스하는 클라이언트 역할 말고도 파일을 업로드/다운로드하는 FTP클라이언트나 메일의 클라이언트 역할을 수행할 수 있기 때문에 여러 종류의 URL이 준비되어 있음
- http:, ftp:, mailto: 는 엑세스하는 방법을 나타내는 프로토콜이라고함 (file: 은 아님)
#### 2. 브라우저는 URL을 해독
- 브라우저는 다음과 같이 URL을 분해하여 해독함
- https:(프로토콜) + // + 웹 서버명(도메인명) + / + 디렉토리 명 + / + 파일명
- 예시)
- (a) http://www.lab.cyber.co.kr/dir/  =>  파일명 생략, /dir/index.html을 엑세스함
- (b) http://www.lab.cyber.co.kr/      =>  파일명 생략, /index.html을 엑세스함
- (c) http://www.lab.cyber.co.kr       =>  파일명, 디렉토리명 생략, /index.html을 엑세스함
- (d) http://www.lab.cyber.co.kr/whatisthis  =>  루트 디렉토리 밑에 whatisthis가 파일인지 디렉토리인지 확인 
- => URL을 해독하면 어디에 엑세스해야 하는지 판명됨

