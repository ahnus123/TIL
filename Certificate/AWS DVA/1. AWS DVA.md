# AWS DVA(AWS Certified Developer - Associate)

## 용어 정리

<img src="https://user-images.githubusercontent.com/33214969/150685706-422ada29-3351-4ef4-a047-61759d0b2ddc.png" alt="AWS summary1.png" width="50%;" />

<img src="https://user-images.githubusercontent.com/33214969/150685711-6c285781-1f9c-49d3-af1a-1e0d1372a69e.png" alt="AWS summary2.png" width="70%;" />

### 용어

1. [서버] EC2
   + IAM = 사용자 계정 및 그룹 관리
2. [DB] RDS
3. [네트워킹] VPC, ELB = L4
4. [스토리지] S3, EBS(=빠른 블록 저장장치), Glacier(=저렴한 스토리지)
5. [Firewall] Security Group
6. [네트워크 ACL] NACL

<br/>

### AWS 기초서비스

1. 컴퓨팅 - 서버와 서버 이중화, 이중화서비스<br/> → EC2 : 서버<br/> → EC2 Container Service : 컨테이너 기반 서버<br/> → ELB(=Elastic Load Balancing) = L4<br/> → Lambda : 이벤트 응답으로 코드 실행<br/> → Auto Sacling : 서버 자동 증설
2. 네트워킹 - 네트워크 구성, DNS, 데이터 전용선<br/>→ VPC : 네트워크<br/> → Route 53 : DNS<br/> → Direct Connect : 데이터 전용선
3. 스토리지 - 데이터 저장공간<br/>→ S3 : 일반 스토리지<br/> → EBS(=Elastic Block Store) : 빠른 블록 저장장치<br/> → Glacier : 저렴한 스토리지<br/> → CloudFront : CDN
4. 관리 및 보안 - 계정관리와 모니터링 시스템<br/>→ IAM : 사용자 계정 및 그룹 관리<br/> → CouldWatch : 모니터링 시스템<br/> → CloudTrail : 계정에 대한 API 호출 기록
5. 애플리케이션 - 애플리케이션 서비스<br/>→ WorkSpaces : 데스트톱을 간편하게 프로비저닝<br/> → WorkDocs : 스토리지 및 공유 서비스

<br/>

### AWS 플랫폼 서비스

1. 데이터베이스<br/>→ RDS : 관계형 DB. MySQL, Oracle, ...<br/> → DynamoDB : NoSQL<br/> → Redishift : DW 설정, 페타규모<br/> → ElasticCache : 캐시 클러스터
2. 분석<br/>→ Kenesis : 데이터 수집처리. 실시간 처리<br/> → EMR : 데이터 프로세싱<br/> → Data Pipeline : 데이터 액세스, 변환 처리
3. 앱 서비스<br/>→ CloudSearch : 검색 기능<br/> → SES : 이메일<br/> → SWF : 상태 추적, 작업 조정
4. 배포 및 관리<br/>→ OpsWorks : 애플리케이션 구성과 배포 자동화<br/> → CloudFormation : 리소스 템플릿 생성<br/> → Elastic Beanstalk : 애플리케이션 자동 배포 처리<br/> → CodeDeploy
5. 모바일 서비스<br/>→ SNS : 알림을 스마트폰으로 푸시<br/> → Cognito : 사용자 데이터 동기화 및 자격증명<br/> → Mobile Analytics : 모바일 분석, 보고서<br/> → ElasticCache : 캐시 클러스터