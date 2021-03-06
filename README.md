
### AWS SAM Application with 4 Config Rules to validate that your AWS Lambda functions follow security best practices
1. Detects functions created through console
2. Detects functions with shared IAM role
3. Detects functions with multiple triggers
4. Detects functions with wildcard action permissions

#### Deployment Instructions

Install dependencies
```shell
npm install --prefix src/
```
Create deployment bucket
```shell
aws s3 mb s3://DEPLOYMENT_BUCKET_NAME --region YOUR_REGION_NAME
```
Package your application
```shell
aws cloudformation package \
   --template-file template.yaml \
   --output-template-file template-packaged.yaml \
   --s3-bucket DEPLOYMENT_BUCKET_NAME
```
Deploy config rules
```shell
aws cloudformation deploy \
   --region YOUR_REGION_NAME
   --template-file template-packaged.yaml \
   --stack-name lambda-config-rules \
   --capabilities CAPABILITY_IAM \
   `# optionally add FunctionShield token to enable protection of custom rules lambda functions (http://bit.ly/2AaBJ3x)` \
   --parameter-overrides FunctionShieldToken=YOUR_FUNCTION_SHIELD_TOKEN
```
