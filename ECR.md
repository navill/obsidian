

ECR 컨테이너 이미지를 계정별로 식별하기 때문에 정해진 형식으로 등록해야함
`aws_account_id.dkr.ecr.ap-northeast-2.amazonaws.com/{repo_name}:{tag}`
```shell
# container reponame 변경
$ AWS_ACCOUNT_ID=$(aws sts get-caller-identity --query 'Account' --output text)
$ docker image tag old_name:v1 ${AWS_ACCOUNT_ID}.dkr.ecr.ap-northeast-2.amazonaws.com/old_name:v1
```


