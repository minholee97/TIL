### 1. 웹 서버의 설치 장소
#### 1. 사내에 웹 서버를 설치하는 경우
- 인터넷을 빠져나와서 서버에 도착하는 과정은 서버의 설치 장소에 따라 다름
- 가장 간단한 방법은 사내의 LAN에 서버를 설치하고 인터넷에서 직접 액세스하는 경우
- 이렇게하면 가장 가까운 POP에 있는 라우터 -> 액세스 회선 -> 서버측 라우터 순으로 경유하면 서버 머신에 도착함
- 이렇게 하면 사내 LAN에 설치한 서버말고 클라이언트 전부 글로벌 주소를 할당해야 하므로 문제가 있음
- 또한 패킷을 차단하지 않으므로 공격당할 확률이 높음
- 방화벽을 사이에 두고 특정 서버의 특정 애플리케이션에 접근하는 패킷만 통과시키고 나머지는 차단하는 방법도 있는데 이 방법이 가장 일반화됨
#### 2. 데이터센터에 웹 서버를 설치하는 경우
- 회사 안에 서버를 설치하는 것이 아닌 프로바이더가 운영하는 데이터센터 시설에 서버를 설치하거나 프로바이더가 소유하는 서버를 빌려쓰는 형태도 있음
- 데이터센터는 프로바이더의 중심부에 있는 NOC에 직접 접속하거나 프로바이더들이 상호 접속하는 IX에 직결되어 있어서 여기에 서버를 설치하면 고속으로 액세스가 가능함
- 서버에 대한 액세스가 많으면 효과적이라고 볼 수 있음
### 2. 방화벽의 원리와 동작
#### 1. 패킷 필터링형이 주류이다
- 서버의 설치 장소와 상관 없이 보통 방화벽이 서버 앞에 있음
- 방화벽의 기본은 특정 서버와 서버안의 특정 애플리케이션에 액세스하는 패킷만 통과시키고 그 외의 패킷은 차단하는 것
- 방화벽은 여러 방법으로 만들 수 있지만 패킷 필터링형이 가장 많이 사용됨
#### 2. 패킷 필터링의 조건 설정 개념
- 패킷의 헤더들은 통신의 제어 정보를 가지고 있음
- 받는 패킷의 IP 헤더의 수신처 IP 주소와 웹 서버의 IP 주소가 일치하는 패킷은 통과 시킴 (송신처도 등록이 가능하면 등록, 보통은 어디에서 오는지 모르기 때문에 불가)
- 마찬가지로 보내는 패킷의 송신처 IP 주소와 웹 서버의 IP 주소가 일치하는 패킷은 통과 시킴
#### 3. 애플리케이션을 한정할 때 포트번호를 사용
- 이렇게 하면 인터넷과 웹 서버 사이를 흐르는 패킷은 전부 통과해서 위험함
- 웹 서버라면 웹 이외의 애플리케이션의 패킷을 전부 차단하는 것이 좋음
- 포트번호는 TCP, UDP 헤더에 기롣되어 있으므로 이 번호가 웹 서버를 뜻하는 80번인지 확인하는 절차를 조건으로 추가
#### 4. 컨트롤 비트로 접속 방향을 판단함
- 최초에 웹 서버에서 인터넷측으로 패킷이 흐르는 것을 방지하기 위해 TCP 컨트롤 비트가 SYN:1, ACK:0인 패킷을 차단함
- 이렇게 IP 주소와 포트 번호, 컨트롤 비트로 패킷을 필터링할 수 있음
- UDP는 접속 단계의 동작이 없기 때문에 컨트롤 비트로 액세스 방향을 판별할 수 없음
#### 5. 사내 LAN에서 공개 서버용 LAN으로 조건 설정
- 사내 LAN과 서버 LAN이 자유롭게 패킷을 주고 받도록 설정했을때 송신처 IP 주소 조건을 설정하지 않으면 인터넷측의 패킷도 유입되므로 주의가 필요함
#### 6. 밖에서 사내 LAN으로 액세스할 수 없음
- 인터넷의 라우터에는 프라이빗 주소를 등록하지 않으므로 주소 변환이 필요하지만 방화벽의 라우터에는 프라이빗 주소를 등록하면 서버 LAN과 사내 LAN 사이에서 패킷 중계 가능
#### 7. 방화벽을 통과
- 패킷 필터링형 방화벽(필터링 기능을 가진 라우터라고 봐도 무방함)은 송, 수신처 IP 주소, 포트 번호, 컨트롤 비트로 패킷을 통과시킬지 판단함
#### 8. 방화벽으로 막을 수 없는 공격
- 일반적인 패킷 필터링은 시점과 종점으로 전송을 판단하기 때문에 패킷의 내용을 보지 않으므로 위험한 패킷인지 모름 (패킷의 문제라기 보다는 서버의 잠재적인 오류로 인해 서버가 다운되는 오류가 발생할 수 있음)
- 그래서 패킷의 내용을 조사해서 위험한 패킷을 차단하는 장치나 소프트웨어를 별도로 준비하는 것이 좋음(방화벽의 옵션으로 제공되는 것도 있음)
###  3. 서버에 Request를 분배해서 서버의 부하 
#### 1. 처리 능력이 부족하면 복수의 서버로 부하 분산
- 서버에 엑세스가 증가하면 회선을 빠르게 하는 것만으로는 부족함
- 고속화된 회선에서 쏟아지는 무수한 패킷을 서버의 처리 능력이 따라잡지 못할 수 있기 때문에 분산 방식이 필요함
- 따라서 복수의 서버를 사용해서 처리를 분담하는 것으로 서버 한 대당 처리량을 줄일 수 있음
- 클라이언트가 보내는 리퀘스트를 웹 서버에 분배하는 방식은 DNS 서버에서 분배하는 것이 간단함
- DNS 서버에 하나의 도메인 명에 여러 IP 주소를 등록하고 조회가 발생할 때마다 차례를 바꿔서 회답(라운드 로빈)
- 다만, 고장난 웹 서버를 피해서 IP 주소를 회답해주면 좋을텐데 보통 DNS서버가 확인하지 못하므로 웹 서버의 동작 여부와 상관없이 IP 주소를 회답함 (최근 브라우저들은 DNS 서버가 회답한 IP 주소들을 앞에서부터 액세스하는데 실패하면 자동으로 다음 주소로 시도하기 때문에 큰 문제가 되지 않음)
- 라운드 로빈 방식에서는 복수의 페이지에 걸쳐서 대화하는 경우 대화가 끊길 수 있으므로 문제가 발생할 수 있음 (예를 들어 주문 페이지가 여러 페이지로 나뉜 경우)
#### 2. 부하 분산 장치를 이용해 복수의 웹 서버로 분할
- 위와 같은 문제를 해결하기 위해 부하 분산 장치 = 로드 밸런서 라는 기기를 사용하게 되었음
- DNS 서버에 도메인 명과 로드 밸런서의 IP 주소를 등록하여 클라이언트가 로드 밸런서에 리퀘스트를 보내도록 유도
- 로드 밸런서는 여러 웹 서버들 중 클라이언트가 보낸 리퀘스트를 어디에 전송해야 할지 판단함
- 판단 근거는 대화가 복수의 페이지에 걸쳐 있는지, 웹 서버의 부하 상태 등이 있음
- 부하 상태의 경우, 웹 서버와 정기적으로 통신해서 CPU나 메모리 사용률을 수집하고 판단할 수도 있고, 시험 패킷을 보내 응답 시간으로 부하를 판단할 수도 있음
- 복수의 페이지에 걸친 경우, 이전의 리퀘스트와 같은 웹 서버에 전송해야 하는데 이를 판단하기 위해 데이터를 보낼때 그 안에 전후의 관련 정보를 부가하거나 HTTP 헤더에 쿠키를 부가하기도 함
- 프록시나 주소 변환 때문에 IP 주소로 판단할 수는 없음
### 4. 캐시 서버를 이용한 서버의 부하 분산
#### 1. 캐시 서버의 이용
- 캐시 서버는 프록시라는 구조를 통해 데이터를 캐시에 저장하는 서버
- 프록시는 웹 서버와 클라이언트 사이에서 중개하는 역할인데 웹 서버에서 받은 데이터를 디스크에 저장하고 웹 서버대신 클라이언트에게 데이터를 반환함 (캐시)
- 캐시 서버에서 리퀘스트를 처리하면 그만큼 웹 서버의 부하가 줄면서 웹 서버의 처리 시간을 단축할 수 있음
#### 2. 캐시 서버는 갱신일로 콘텐츠를 관리함
