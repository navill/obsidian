
### CodeCommit 연결 시 access error(403)

- 루트 사용자가 아닌 git에 접근하기 위한 사용자를 생성해야함
- 사용자 -> 권한 -> AWSCodeCommitPowerUser 권한 추가
- 사용자 -> 보안 자격 증명 -> AWS CodeCommit에 대한 HTTPS Git 자격 증명 -> 자격 증명 생성
	- 자격 증명 아이디, 패스워드 확인
- cloud9 콘솔에서 `git config --global credential.helper manager` 입력 후 git push
- username, password를 방금 얻어온 정보로 입력


