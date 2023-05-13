# 코드 자동 배포

## CI(Continuous Integration - 지속적인 통합)

* 코드 버전 관리를 하는 VCS시스템(Git, SVN 등)에 PUSH가 되면 자종으로 테스트와 빌드가 수행되어 안정적인 배포 파일을 만드는 과정

### CI [4가지 규칙](https://www.martinfowler.com/articles/originalContinuousIntegration.html)

* 모든 소스 코드가 있는 단일한 장소를 유지하고 누구든 현재의 소스 코드 및 이전 버전을 얻을 수 있는 곳으로 유지합니다.
* 빌드 프로세스를 자동화하여 누구나 단일 명령으로 시스템을 소스 코드에서 빌드할 수 있도록 합니다.
* 테스트를 자동화하여 언제든지 단일 명령으로 시스템에 대한 품질 좋은 테스트 스위트를 실행할 수 있도록 합니다.
* 누구나 최신의 실행 파일을 얻을 수 있도록 하며, 그 실행 파일이 지금까지의 최상의 실행 파일임을 확신합니다.

### 도구

* Travis CI
* 젠키스
  * 설치형이여서 EC2 인스턴스 하나 더 필요함
* CodeBuild
  * 유료

## 연결 단계

### 1. Travis CI와 S3을 연동

* CodeDeploy는 저장 기능이 없습니다. 그래서 Travis CI가 빌드한 결과물을 받아서 AWS S3을 저장한다.

1. AWS Key 발급
   * 일반적으로 AWS 서비스에 외부 서비스가 접근할 수 없습니다. 그러므로 접근 가능한 권한을 가진 key를 생성가하고 TravisCI가 AWS의 S3와 CodeDeploy에 접근할 수 있도록 한다.
2. Travis CI에 키 등록
   * `.travis.yaml` 파일에서 등록한 값을 사용할 수 있다. 쉘 스크립트에서 변수를 사용했던 것과 비슷하다.
   * 예 `$AWS_ACCESS_KEY`
3. S3 버킷 생성

### 2. Travis CI와 AWS S3, CodeDeploy 연동

1. EC2에 IAM 역할 추가하기
   * 역할
     * AWS 서비스에만 할당할 수 있는 권한
     * EC2,CodeDeploy,SQS 등
   * 사용자
     * AWS 서비스 외에 사용할 수 있는 권한
     * 로컬 PC,IDC 서버 등
2. CodeDeploy 에이전트 설치

```bash
# CodeDeploy 에이전트 설치 파일 이동
# AWS CLI를 사용하여 "s3://aws-codedeploy-ap-northeast-2/latest/install" 경로의 파일을 
# "." 현재 디렉토리로 복사하는 명령어
aws s3 cp s3://aws-codedeploy-ap-northeast-2/latest/install . --region ap-northeast02

# install 파일에 실행 권한 추가
chmod +x ./install

# install 파일로 설치
sudo ./install auto

# Agent 실행상태 확인
sudo service codedeploy-agent status
```

{% hint style="info" %}
"service"는 Linux 시스템에서 서비스(데몬)를 관리하는 명령어입니다.



service <서비스 이름> <동작>



일반적으로 사용되는 동작은 다음과 같습니다:

* `start`: 서비스를 시작합니다.
* `stop`: 서비스를 중지합니다.
* `restart`: 서비스를 다시 시작합니다.
* `status`: 서비스의 상태를 확인합니다.
* `reload`: 서비스를 다시 불러옵니다. (설정 파일 변경 사항 적용)
{% endhint %}

3. CodeDeploy를 위한 권한 생성
   * CodeDeploy에서 EC2에 접근하려면 권한이 필요합니다.
4. CodeDeploy 생성
5. Travis CI,S3,CodeDeploy 연동

## Refrence

> :books:[스프링 부트와 AWS로 혼자 구현하는 웹 서비스](https://product.kyobobook.co.kr/detail/S000001019679)
