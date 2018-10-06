In us-west-2 region (Oregon) 
Petstore API is deployed as a Regional API. Method is mocked.
Performance test with Locust at 10 req/sec.

From desktop in Montreal : median=88msec / 95percentile=95msec
From us-west-2 ec2 instance : median=**7msec** / 95percentile=9msec
From ca-central-1 ec2 instance : median=86msec / 95percentile=88sec

Observations:
-extremely low latencey (< 10msec) when client is in the same AWS region
- AWS Network does not provide a significant latency gain from Montreal to the us-wset-2 region

Sane tests, but the API is set as Edged Optimized
