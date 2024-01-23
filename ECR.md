

ECR 컨테이너 이미지를 계정별로 식별하기 때문에 정해진 형식으로 등록해야함
`aws_account_id.dkr.ecr.ap-northeast-2.amazonaws.com/{repo_name}:{tag}`
```shell
# container reponame 변경
$ AWS_ACCOUNT_ID=$(aws sts get-caller-identity --query 'Account' --output text)
$ docker image tag old_name:v1 ${AWS_ACCOUNT_ID}.dkr.ecr.ap-northeast-2.amazonaws.com/old_name:v1

# 레지스트리 인증
$ aws ecr --region ap-northeast-2 get-login-password | docker login --username AWS --password-stdin https://${AWS_ACCOUNT_ID}.dkr.ecr.ap-northeast-2.amazonaws.com/sbcntr-backend
```

- An error occurred (ExpiredTokenException) when calling the GetAuthorizationToken operation: The security token included in the request is expired 오류 발생 시 -> aws configure에서 엑세스 아이디 & 키 재등록

