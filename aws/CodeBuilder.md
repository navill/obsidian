- buildspec.yml 코드 생성(소스 루트에 위치)
```yml
version: 0.2

env:
	variables:
		AWS_REGION_NAME: ap-northeast-2
		ECR_REPOSITORY_NAME: sbcntr-backend
		DOCKER_BUILDKIT: "1"

phases:
	install:
		runtime-versions:
			docker: 19

pre_build:
	commands:
		- AWS_ACCOUNT_ID=$(aws sts get-caller-identity --query 'Account' --output text)
		- aws ecr --region ap-northeast-2 get-login-password | docker login --username AWS --password-stdin https://${AWS_ACCOUNT_ID}.dkr.ecr.ap-northeast-2.amazonaws.com/sbcntr-backend

		- REPOSITORY_URI=${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION_NAME}.amazonaws.com/${ECR_REPOSITORY_NAME}
		# 태그 이름에 Git 커밋 해시를 이용
		- IMAGE_TAG=$(echo ${CODEBUILD_RESOLVED_SOURCE_VERSION} | cut -c 1-7)

build:
	commands:
		- docker image build -t ${REPOSITORY_URI}:${IMAGE_TAG} .

post_build:
	commands:
		- docker push ${REPOSITORY_URI}:${IMAGE_TAG}
```

### codebuild role
- (default) CodeBuildBasePolicy-...
- ecr 권한 추가
```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "ListImagesInRepository",
            "Effect": "Allow",
            "Action": [
                "ecr:ListImages"
            ],
            "Resource": [
                "arn:aws:ecr:ap-northeast-2:975141803454:repository/sbcntr-backend",
                "arn:aws:ecr:ap-northeast-2:975141803454:repository/sbcntr-frontend"
            ]
        },
        {
            "Sid": "GetAuthorizationToken",
            "Effect": "Allow",
            "Action": [
                "ecr:GetAuthorizationToken"
            ],
            "Resource": "*"
        },
        {
            "Sid": "ManageRepositoryContents",
            "Effect": "Allow",
            "Action": [
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
            "Resource": [
                "arn:aws:ecr:ap-northeast-2:975141803454:repository/sbcntr-backend",
                "arn:aws:ecr:ap-northeast-2:975141803454:repository/sbcntr-frontend"
            ]
        }
    ]
}
```