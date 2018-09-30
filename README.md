# aws-notes
## Performance testing Amazon API Gateway and AWS Lambda
- with authorization set to NONE, API Gateway introduces minimal latency. < 10msec. Tests should be run to check how much latency is added by Cognito Authorizer, Custom Authorizer or IAM Authentication.
- a Lambda function acting as a HTTP proxy adds a 200 msec latency. Even if containers are warm.
- Lambda function cold starts add latency for the first requests. Up to a few seconds.
- cold starts for Lambda function **with VPC access** take more time.
- cold starts only affect the first requests. Once the container is warm, there's no additional latency. Even for Lambda accessing VPCs.
- when it comes to load: the first bottleneck is the limit of **Lambda concurrent executions**. **1000 per account** per default. Can be increased by contacting AWS support. Assuming a backend system takes 1 sec to process a request, this is a limit of 1000 req/sec.
- if the limit of concurrent Lamda functions execution is exceeded then requests are **throttled**. Which means discarded for synchronous Lambda invocation. Can be monitored with the throttles metric in CloudWatch.
- for Lambda functions accessing VPCs, the next bottleneck is the **limit of ENIs per account**. One ENI can handle 3GB of Lambda functions memory according to the documentation. See https://docs.aws.amazon.com/lambda/latest/dg/concurrent-executions.html#concurrent-execution-safety-limit. In my tests it looked like it is less than that. Around 1.5 GB. With 128MB memory functions, 80 concurrent executions required 8 ENIs to be created. 400 concurrent executions required 40 ENIs to be created. The limit of **ENIs per account being 350 by defaut**, this translates to **around 3500 concurent executions of 128MB Lambda functions**.
- for Lambda functions accessing VPCs, the next bottleneck is the number of available IP addresses in your subnets. You can associate multiple Subnets to a Lambda function. Lambda creates ENIs in Subnets equally, e.g. if 40 ENIs are created, 20 ENIs will be created in AZ #1 and 20 other ENIs in AZ #2. If you have **two /24 Subnets you have roughly 500 IPs addresses available** which translates into around **5000 concurrent executions of 128MB Lambda functions**. 

Actions suggested to support the load:
- ask AWS support to increase Lambda concurrent executions from 1000 to 5000.
- ask AWS support to increase number of ENIS from 350 to 2000.
- create additional 2 Subnets = 500 additional IPs. 

Notes:
- backend systems will probably be the bottlenecks, before AWS services are.
- the load will take weeks to ramp up. Will provide time to monitor and adjust if required.
