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

## 엔드포인트 설정

#### 게이트웨이 엔드포인트
- VPC -> 엔드포인트 탭에서 엔드포인트 추가
- 서비스 탭에서 s3(gateway유형) 선택
	![](Users/navill/Documents/Obsidian%20Vault/Pasted%20image%2020240117184430.png)
- 적용하려는 VPC와 연결이 필요한 라우팅 테이블(퍼블릭, 프라이빗) 선택
- 설정 완료 후 라우팅 테이블 설정으로 가서 퍼블릭, 프라이빗 라우팅 테이블에 엔드포인트가 추가되었는지 확인
	![](Users/navill/Documents/Obsidian%20Vault/Pasted%20image%2020240117184623.png)
	![](Users/navill/Documents/Obsidian%20Vault/Pasted%20image%2020240117184706.png)

- 프라이빗 EC2에 접속해서 ping 시도 -> 성공
	![](Users/navill/Documents/Obsidian%20Vault/Pasted%20image%2020240117184851.png)
	![](Users/navill/Documents/Obsidian%20Vault/Pasted%20image%2020240117185508.png)


#### 인터페이스 엔드포인트
- 프라이빗 EC2에서 cloudformation dns에 대한 ping 시도 -> 실패
	- 외부 인터넷 접근이 불가능한 상태(프라이빗 링크 연결 X)

> **AWS 서비스는 리전별 기본 DNS 호스트 주소를 가지고 있음**
> -> VPC 인터페이스 엔드포인트를 생성하면 엔드포인트 전용 DNS 호스트가 생성됨
> 
> 인터페이스 엔트포인트 설정값 중 `프라이빗 DNS 활성화` 여부에 따른 통신 흐름
> - 프라이빗 DNS 비활성화
> 	- 기본 DNS 호스트: 인터넷 구간을 통한 퍼블릭 통신
> 	- 엔드포인트 전용 DNS 호스트: 인터페이스 엔드포인트를 통한 프라이빗 통신
> - 프라이빗 DNS 활성화
> 	- 기본 DNS 호스트: 인터페이스 엔드포인트를 통한 프라이빗 통신
> 	- 엔드포인트 전용 DNS 호스트: 인터페이스 엔드포인트를 통한 프라이빗 통신


