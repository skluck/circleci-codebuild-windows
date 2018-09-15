## Circle CI + AWS CodeBuild

Use AWS CodeBuild to enable Windows Builds from Circle CI.

1. Create `ECR` with windows image, `service-linked role`, `S3 bucket` for transfer.

2. Create CodeBuild Project:
    > https://docs.aws.amazon.com/cli/latest/reference/codebuild/create-project.html
    > ```
    > aws codebuild create-project \
    > --name "circleci-windows" \
    > --source "type=NO_SOURCE" \
    > --environment "type=WINDOWS_CONTAINER,image=aws/codebuild/windows-base:1.0,computeType=BUILD_GENERAL1_MEDIUM" \
    > --service-role "arn:aws:iam::account-ID:role/CodeBuild-service-role" \
    > --artifacts "type=NO_ARTIFACTS" \
    > # optional - for VPC access
    > --vpc-config "vpcId=vpc-1234,subnets=subnet-1234,securityGroupIds=sg-1234,sg-5678"
    > ```

3. Run project:
    > https://docs.aws.amazon.com/cli/latest/reference/codebuild/start-build.html
    > ```
    > aws codebuild start-build \
    > --project-name "circleci-windows" \
    > --source-type-override S3 \
    > --source-location-override "bucket/1234/input.zip" \
    > --artifacts-override "type=S3,location=bucket,path=1234,name=output.zip,packaging=ZIP,encryptionDisabled=true"
    > --image-override "1234567890.dkr.ecr.us-east-1.amazonaws.com/repo_name/image_name:tag_name"
    > ```
