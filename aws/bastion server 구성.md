### public server(bastion)
- public subnet에 구성될 서버

### private server
- private subnet에 구성될 서버
	- RDS와 같이 프라이빗 서브넷에 구성된 서버는 외부에서 접근이 불가능해야함
	- 만약 IDE를 이용한 db(RDS) 접근이 필요할 경우 bastion을 거쳐서 private으로 접근 가능

