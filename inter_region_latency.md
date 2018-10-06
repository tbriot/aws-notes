Testing with the Amazon API Gateway
Petstore API is deployed. Method is mocked.
Performance test with Locust at 10 req/sec.

# In us-west-2 region (Oregon)
## Regional API
From desktop in Montreal : median=**88ms** / 95percentile=95ms
From us-west-2 ec2 instance : median=**7ms** / 95percentile=9ms
From ca-central-1 ec2 instance : median=**86ms** / 95percentile=88ms

Observations:
- extremely low latencey (< 10ms) when client is in the same AWS region
- AWS Network does not provide a significant latency gain from Montreal to the us-wset-2 region

## Edged Optimized API
From desktop in Montreal : median=**88ms** / 95percentile=310ms??
From us-west-2 ec2 instance : median=**26ms** / 95percentile=42ms??
From ca-central-1 ec2 instance : median=**85ms** / 95percentile=220ms??

Observations:
- latency jumps from 7 to 26msec in the us-west-2 region
- does not improve latency when client is in Montreal. Confirms that the AWS Network does not provide latency gains between MTL and US-WEST-2

# In ap-southeast-2 region (Sydney)
## Regional API
From desktop in Montreal : median=**300ms** / 95percentile=320ms
From us-west-2 ec2 instance : median=**200ms** / 95percentile=220ms
From ca-central-1 ec2 instance : median=**170ms** / 95percentile=170ms

Observations:
- AWS Network does improve latency (200ms versus 300ms) between MTL and Sydney

Resources:
- https://www.cloudping.co/
- https://www.cloudping.info/

