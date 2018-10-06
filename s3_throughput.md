Moving a 274MB file around.
ISP plan provides 30MB/s download and 10MB/s upload

## Upload
Desktop in Montreal to bucket in ca-central-1 : 1.4MB/s  
Desktop in Montreal to bucket in us-west-2 : 1.4MB/s
## Inter-region
bucket in ca-central-1 to bucket in us-west-2 : **> 17MB/s**
## Download
bucket in ca-central-1 to desktop in Montreal : > 4 MB/s  
bucket in us-west-2 to desktop in Montreal : 3.5 MB/s

## Observations
- AWS Network has a huge bandwith, 10x my upload speed 

Resources:
- http://www.speedtest.net
