![](Users/navill/Documents/Obsidian%20Vault/Pasted%20image%2020240116221323.png)
- NAT 게이트웨이는 반드시 퍼블릿 서브넷에 위치
- 다른 VPC 영역 또는 AWS 퍼블릭 서비스를 이용할 경우, [VPC 엔드포인트](VPC%20엔드포인트.md)를 구성해야함

### public server(bastion)
- public subnet에 구성될 서버

### private server
- private subnet에 구성될 서버
	- RDS와 같이 프라이빗 서브넷에 구성된 서버는 외부에서 접근이 불가능해야함
	- 만약 IDE를 이용한 db(RDS) 접근이 필요할 경우 bastion을 거쳐서 private으로 접근 가능

---

### ICMP를 이용한 네트워크 연결 확인(ping)
- ICMP(ping)을 이용해 private subnet의 상태 확인
	- 보안 그룹에서 인바운드 규칙에 ICMP 설정 추가
	- 소스에는 bastion의 보안 그룹을 추가
	![](Users/navill/Documents/Obsidian%20Vault/Pasted%20image%2020240116183329.png)
- bastion에서 ping <private ec2의 private ip>로 ping 전달
	![](Users/navill/Documents/Obsidian%20Vault/Pasted%20image%2020240116183515.png)
