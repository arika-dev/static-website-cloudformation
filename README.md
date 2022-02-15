# static-website-cloudformation

## Create Infrastructure

***Step 1***
Create a bucket to store cloudformation templates
```shell
 aws s3 mb s3://cf-example-dev
 
``` 
***Step 2***
Package template

```shell
aws --region us-east-1 cloudformation package \
    --template-file ./infrastructure/main.yaml \
    --s3-bucket cf-example-dev \
    --output-template-file packaged.template
```

***Step 3***
Deploy

```shell
aws --region us-east-1 cloudformation deploy \
    --stack-name cf-example-dev-stack \
    --template-file packaged.template \
    --capabilities CAPABILITY_NAMED_IAM CAPABILITY_AUTO_EXPAND \
    --parameter-overrides  DomainName=example.com SubDomain=www HostedZoneId=<ROUTE_53_HOSTED_ZONE_ID>
```

## Deploying website
Assuming `public` folder contains your artefacts, you can deploy the content using 
```shell
aws s3 sync ./public s3://www.example.com
```