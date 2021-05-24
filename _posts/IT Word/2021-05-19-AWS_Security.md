---
layout: post
title: "Security Engineering on AWS 교육 정리"
categories: IT
tags: word
---

## Security Engineering on AWS 강의 중 주요사항 정리

### AWS 계정 루트 사용자, IAM사용자
루트계정은 처음에 만들때만 사용(Multi-Factor인증 활성화)하고, 그 이후에는 최소한의 권한을 부여한 IAM계정을 사용하자

### IAM 사용자는
Accesskey ,security key 두쌍을 만들 수 있다. (Active/Inactive 가능)
**access-key, private key는 가능한 프로그램에 넣어서 사용하지 않기를 권장 (git에 올려넣었다가 유출될 가능성)**
inactive가 있는이유? 유출되어었을때 삭제하지말고 inactive하고, cloudtrail에가서 유출된 키를 검색해서 찾아볼 수 있음.
키 교체시 기존키 유예기간을 위해 inactive

#### 그러면 어떻게 사용할까?  (role을 사용하여 접근한다.)
* STS(Security Token Service)로 부터 임시 키값을 요청하고 받아쓴다.
>  aws sts get-session-token --duration-seconds 900 
본인의 토큰값

*  assume role, 특정 리소스 액세스 권한을 얻기위한 임시 자격증명을 요청한다.
> aws sts assume-role  --role-arn arn:aws:iam::112434925628:role/AssumeRole_S3_jkcho --role-session-name  AssumeRole_S3_jkcho_1 --duration 900
iam계정 112434925628의 role이라는 리소스 중 AssumeRole_S3_jkcho를 원한다.

* 세션정책
> aws sts assume-role  --role-arn arn:aws:iam::112434925628:role/AssumeRole_S3_jkcho --role-session-name  AssumeRole_S3_jkcho_1 --duration 900  --policy-arns arn=arn:aws:iam::aws:policy/AmazonS3FullAccess

### IAM정책 + 리소스정책(S3의경우 버킷, ACL정책, 리소스는기본적으로 열려있음) + 세션정책
#### 리소스정책이 따로 있는 이유는?
IAM사용자가 천명인데, 특정 데이터 접근을 제한할때는 리소스정책에서 제어가능(데이터측면의 제어가 필요할떄!)
#### 정책 체크 순서
리소스기반정책 -> 세션정책 -> IAM정책 순으로 체크
리소스가 퍼블릭 오픈되어있을 수 있음으로 가장먼저 체크

### 정책 (Policy)
allow를 하면 그외에는 deny이다. 하지만 관리차원에서 deny도 명시해주는것이 좋다.
인라인 정책 보다는 고객 관리형 정책을 사용하자: 고객 하나하나 들어가서 정책을 넣는게 아니라, polices를 통해서 권한을 만들고 롤을통해부여

### Permission boundary
 - iam role을 변경할 수 있는 권한의 바운더리를 치는 것

### cloud trail
cloud trail log는 삭제에 대해 mfa 설정을 권장, 그리고 또 별도의 계정으로 또 모아관리?
cloud trail은 api로그만 저장됨, 외부IP로 ssh접속하는 등 (비API)로깅은 되지 않는다.

### 객체잠금 
 - worm처럼 사용
 - 규정준수 모드(아무도 수정 못함), 

### System Manager Automation vs 패치매니저
기존에는 ssm automation으로 패치를 관리했었다. 그 이후에 패치머니저가 나왔다.
패치매니저는  패치를 종합적으로 관리할 수 있다.
패치를 자동패치할때 유예기간 설정 등 할 수 있다.

### VPC Flow log
네트워크 로그같은것 (데이터는 빠져있음) -> 트래픽미러링은 데이터를 볼 수 있음(wire shark같은)
vpc flow log -> cloudwatch logs로 보낼 수 있음, 연계해서 확인 가능

### s3서버 액세스 로그
S3에 접근방법은 두가지: aws api, http
그 중 aws api는 cloudtrail log에 남지만 http는 안남는다
예를들어, Public access 같은경우 http로접근
모든 로그를 확인하기 위해 S3 액세스 로그 확인 필요

### ELB(Elastic Load Balancer) 로그
client ip, port, 응답시간, 요청내용, 인증서 등등 elb 로그에 남고 s3로 저장

### 실시간 로그 관리는 Kinesis, 분석은 glue, athena
Kinesis Data Stream vs Kinesis Data Firehose
kinesis로 쌓고 glue로 스키마 추출하고 athena SQL로 분석한다

## 확인이필요한 사항?
계정과 사용자의 차이?
### 계정?
아마존 웹 서비스에 가입하면 하나의 계정이 만들어집니다. 이 때 만들어지는 사용자를 루트 사용자라고 부릅니다. 루트 사용자를 사용해서 아마존 웹 서비스에서 제공하는 모든 서비스를 곧바로 이용할 수 있습니다. EC2 가상 컴퓨팅 자원을 사용할 수도 있고, RDS 데이터베이스를 만들거나, 오브젝트 스토리지 S3 버킷에 데이터를 저장하는 것도 가능합니다. EC2 머신, RDS 데이터베이스, S3 버킷과 같은 것들을 자원(리소스)이라고 합니다. 루트 사용자는 계정에 속한 모든 리소스들에 대한 무제한적인 접근 권한을 가집니다.
계정 = 루트사용자
하나의 루트사용자, 다수의 IAM사용자
결국, 공용계정 사용이슈이다.

### arn?
Amazon Resource Number
arn의 구성: ARN/파티션구분자/서비스명/리전/계정번호 + 서비스마다의 상세 구분자

### role?
RemoteSecurityAudit -> CLIAssumeRole

### 강사 공유해준 링크
- API 사용 예문: http://dfr20cpm485ln.cloudfront.net/Dev_HLS_index.html
- Lab Video: http://dfr20cpm485ln.cloudfront.net/SecEng_HLS_KO_index.html