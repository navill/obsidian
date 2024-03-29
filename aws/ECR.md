- ECR 정책: https://docs.aws.amazon.com/ko_kr/AmazonECR/latest/userguide/security_iam_id-based-policy-examples.html
- ECR 레포 엑세스 정책 예시: https://docs.aws.amazon.com/ko_kr/AmazonECR/latest/userguide/security_iam_id-based-policy-examples.html#security_iam_id-based-policy-examples-access-one-bucket
```
{
   "Version":"2012-10-17",
   "Statement":[
      {
         "Sid":"ListImagesInRepository",
         "Effect":"Allow",
         "Action":[
            "ecr:ListImages"
         ],
         "Resource":"arn:aws:ecr:ap-northest-2:123456789012:repository/my-repo"
      },
      {
         "Sid":"GetAuthorizationToken",
         "Effect":"Allow",
         "Action":[
            "ecr:GetAuthorizationToken"
         ],
         "Resource":"*"
      },
      {
         "Sid":"ManageRepositoryContents",
         "Effect":"Allow",
         "Action":[
                "ecr:BatchCheckLayerAvailability",
                "ecr:GetDownloadUrlForLayer",
                "ecr:GetRepositoryPolicy",
                "ecr:DescribeRepositories",
                "ecr:ListImages",
                "ecr:DescribeImages",
                "ecr:BatchGetImage",
                "ecr:InitiateLayerUpload",
                "ecr:UploadLayerPart",
                "ecr:CompleteLayerUpload",
                "ecr:PutImage"
         ],
         "Resource":"arn:aws:ecr:ap-northest-2:123456789012:repository/my-repo"
      }
   ]
}
```



ECR 컨테이너 이미지를 계정별로 식별하기 때문에 정해진 형식으로 등록해야함
`{aws_account_id}.dkr.ecr.ap-northeast-2.amazonaws.com/{repo_name}:{tag}`
```shell
# container reponame 변경
$ AWS_ACCOUNT_ID=$(aws sts get-caller-identity --query 'Account' --output text)
$ docker image tag old_name:v1 ${AWS_ACCOUNT_ID}.dkr.ecr.ap-northeast-2.amazonaws.com/old_name:v1

# 레지스트리 인증
$ aws ecr --region ap-northeast-2 get-login-password | docker login --username AWS --password-stdin https://${AWS_ACCOUNT_ID}.dkr.ecr.ap-northeast-2.amazonaws.com/repo_name
```

- An error occurred (ExpiredTokenException) when calling the GetAuthorizationToken operation: The security token included in the request is expired 오류 발생 시 -> aws configure에서 엑세스 아이디 & 키 재등록


ECR 구현을 위한 3개의 엔드포인트
- com.amazonaws.ap-northeast-2.ecr.api: aws cli용
- com.amazonaws.ap-northeast-2.ecr.dkr: docker image push용
- com.amazonaws.ap-northeast-2.s3: static 파일