- 격리된 프라이빗 서브넷에 생성된 자원에 프라이빗 네트워크 통신을 이용하기 위함(보안 목적)
#### 종류
- 엔드포인트([docs](https://docs.aws.amazon.com/ko_kr/vpc/latest/privatelink/aws-services-privatelink-support.html)): AWS 퍼블릭 서비스 대상 프라이빗 연결
	- 게이트웨이 엔드포인트([docs](https://docs.aws.amazon.com/ko_kr/vpc/latest/privatelink/gateway-endpoints.html)): S3, DynamoDB
	- 인터페이스 엔드포인트([docs](https://docs.aws.amazon.com/ko_kr/vpc/latest/privatelink/create-interface-endpoint.html)): 위 대상 외에 나버지 AWS 퍼블릭 서비스
	- [describe-vpc-endpoint-services](https://docs.aws.amazon.com/ko_kr/vpc/latest/privatelink/aws-services-privatelink-support.html#vpce-view-available-services) 명령어로 VPC 엔드포인트를 지원하는 AWS 서비스 목록 확인
- 엔드포인트 서비스([docs](https://docs.aws.amazon.com/ko_kr/vpc/latest/privatelink/create-endpoint-service.html)): 사용자 지정 서비스 대상 연결

