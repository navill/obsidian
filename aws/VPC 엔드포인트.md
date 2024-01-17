- 격리된 프라이빗 서브넷에 생성된 자원에 프라이빗 네트워크 통신을 이용하기 위함(보안 목적)
#### 종류
- 엔드포인트([docs](https://docs.aws.amazon.com/ko_kr/vpc/latest/privatelink/aws-services-privatelink-support.html)): AWS 퍼블릭 서비스 대상 프라이빗 연결
	- 게이트웨이 엔드포인트([docs](https://docs.aws.amazon.com/ko_kr/vpc/latest/privatelink/gateway-endpoints.html)): S3, DynamoDB
	- 인터페이스 엔드포인트([docs](https://docs.aws.amazon.com/ko_kr/vpc/latest/privatelink/create-interface-endpoint.html)): 위 대상 외에 나버지 AWS 퍼블릭 서비스
	- [describe-vpc-endpoint-services](https://docs.aws.amazon.com/ko_kr/vpc/latest/privatelink/aws-services-privatelink-support.html#vpce-view-available-services) 명령어로 VPC 엔드포인트를 지원하는 AWS 서비스 목록 확인
		- `aws ec2 describe-vpc-endpoint-services --filters Name=service-type,Values=Interface Name=owner,Values=amazon --region ap-northeast-2 --query ServiceNames`
- 엔드포인트 서비스([docs](https://docs.aws.amazon.com/ko_kr/vpc/latest/privatelink/create-endpoint-service.html)): 사용자 지정 서비스 대상 연결
- 인터페이스 엔드포인트와 엔드포인트 서비스를 프라이빗 링크(private link)라고 부름

![](Users/navill/Documents/Obsidian%20Vault/Pasted%20image%2020240117183725.png)
- 퍼블릭 EC2에서 S3 DNS로 핑을 찍어보면 성공
- 프라이빗 EC2에서 S3 DNS로 핑을 찍어보면 실패

### 엔드포인트 설정
- VPC -> 엔드포인트 탭에서 엔드포인트 추가
- 서비스 탭의 필터에 s3 -> gateway