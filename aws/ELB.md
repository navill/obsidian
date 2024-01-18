ELB: AWS에서 제공하는 로드 밸런싱 기술
- 리스너: 프로토콜 및 포트를 사용해 연결요청을 확인하는 프로세스
	- 부하 분산 처리를 위한 서비스 정의
- 타겟 그룹: 하나 이상의 대상을 라우팅하여 부하 분산
	- 타겟 그룹에 속한 대상에 대해 주기적인 헬스 체크 및 데이터 전달

#### Application Load Balancer(ALB)
- HTTP/HTTPS와 같이 웹 어플리케이션에 대한 분산 처리
- NLB에 상대적으로 느림
- OSI 7 계층 부하 분산

#### Network Load Balancer(NLB)
- TCP/UDP 프로토콜에 대한 포트 정보를 정의한 네트워크 기반 분산 처리
- ALB에 상대적으로 빠름
- OSI 4 계층 부하 분산
#### Classic Load Balancer(CLB)
- 구 세대 로드 밸런서(잘 안씀)
	
