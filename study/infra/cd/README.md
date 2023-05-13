# 무중단 배포

## CD(Continuous Deployment - 지속적인 배포)

* Continuous Deployment(지속적인 배포)은 소프트웨어 개발 및 배포 과정에서 자동화와 지속적인 통합을 통해 소프트웨어의 변경 사항을 빠르게, 자주, 안정적으로 실제 운영 환경에 배포하는 방법입니다. 이것은 Agile 및 DevOps 개발 방법론의 일환으로 사용되며, 릴리즈 주기를 단축하고 사용자 요구 사항을 신속하게 반영하기 위해 도입됩니다.
* Continuous Deployment에서는 소스 코드 변경이 이루어지면, 자동화된 빌드, 테스트, 배포 프로세스를 통해 변경 사항을 실시간으로 릴리즈에 반영합니다. 개발자들은 작은 단위의 변경 사항을 빠르게 배포하여 실제 사용자들의 피드백을 받고, 지속적인 개선과 배우기를 추구할 수 있습니다.

## 배포 방식

* AWS에서 블루 그린(Blue-Green) 무중단 배포
* 도커를 이용한 웹서비스 무중단 배포
* [엔진엑스](nginx.md)를 이용한 무중단 배포

## 엔진엑스를 이용한 무중단 배포 예시

{% content-ref url="../ci.md" %}
[ci.md](../ci.md)
{% endcontent-ref %}

### 구조

* 엔진엑스는 80(http),443(https)포트를 할당합니다.
* 스프링 부트1은 8081포트로 실행합니다.
* 스프링 부트2는 8082포트로 실행합니다.

### 운영 과정

1. 사용자는 서비스 주소로 접속합니다.(80 혹은 443 포트)
2. 엔직엑스는 사용자의 요청을 받아 현재 연결된 스프링 부트로 요청을 전달합니다.
   * 스프링 부트1 즉, 8081 포트로 요청을 전달한다고 가정합니다.
3. 스프링 부트2는 엔진엑스와 연결된 상태가 아니니 요청받지 못합니다.

### 엔진엑스 설치

```bash
# 엔진엑스 설치
sudo yum install nginx

# 엔진엑스 실행
sudo service nginx start
```

### 엔진엑스 설정 파일 수정

```bash
# /etc/nignx/conf.d/ 에 Nginx 웹 서버의 구성 파일들이이 모여있다.
# 엔진엑스 설정 파일 추가 
sudo vim /etc/nignx/conf.d/service-url.inc
```

추가 내용

<pre class="language-bash"><code class="lang-bash"><strong># "127.0.0.1:8080"은 로컬 호스트(localhost)의 IP 주소와 포트 번호를 나타냅니다
</strong># Nginx의 설정 문법을 사용하여 변수 "$service_url"에 값 "http://127.0.0.1:8080"을 할당하는 구문입니다.
<strong>set $service_url http://127.0.0.1:8080;
</strong></code></pre>

```bash
# 엔진엑스 설정 파일 수정 명령
sudo vim /etc/nginx/nginx.conf
```

수정 내용

<pre class="language-bash"><code class="lang-bash"><strong># load configuraion files for the default server block.
</strong><strong>
</strong><strong># 이는 서버 블록 설정에 대한 추가 구성 파일을 포함할 수 있는 방법입니다.
</strong>include /etc/nginx/conf.d/service_url.cnf;

# location / { ... }: 웹 서버의 기본 서버 블록에 대한 설정을 나타냅니다. 
# 이 설정은 클라이언트로부터의 모든 요청에 대해 적용됩니다.
location / {
# 요청을 $service_url으로 전달하는 프록시 역할을 합니다. 
# 이는 웹 서버가 클라이언트 요청을 다른 서버(예: 애플리케이션 서버)로 전달할 수 있도록 합니다.
    proxy_pass $service_url;
# proxy_set_header: 프록시 서버로 전달되는 요청 헤더를 설정하는 지시문입니다. 
# 이 코드는 X-Real-IP, X-Forwarded-For, Host 등의 헤더를 설정하여 프록시 서버가 원본 요청 정보를 
# 전달할 수 있도록 합니다.
    # "$remote_addr"는 현재 요청을 보내는 클라이언트의 IP 주소를 나타냅니다.
    proxy_set_header X-Real-IP $remote_addr;
    # "$proxy_add_x_forwarded_for"는 프록시 서버에서 전달된 요청의 
    # X-Forwarded-For 헤더 값을 추가하는 역할을 합니다.
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    # "$http_host"는 현재 요청을 받은 호스트의 이름을 나타냅니다.
    proxy_set_header Host $http_host;
}
</code></pre>

## 배포 스크립트들 작성 예시

### appspec.yml

* AWS CodeDeploy의 배포 구성 파일

```yaml
# version: 배포 구성 파일의 버전을 지정합니다. 여기서는 "0.0"으로 설정되어 있습니다.
version: 0.0
# 배포할 운영 체제를 지정합니다. 여기서는 "linux"으로 설정되어 있습니다.
os: linux
# 파일 복사 관련 설정을 지정합니다. 소스 경로에서 대상 경로로 파일을 복사하며, 필요한 경우 덮어쓸 수 있습니다.
files:
  - source:  /
    destination: /home/ec2-user/app/step3/zip/
    overwrite: yes

# 파일 및 디렉토리 권한을 설정합니다. 
# 해당 예시에서는 모든 파일과 디렉토리를 ec2-user 소유자와 ec2-user 그룹으로 설정합니다.
permissions:
  - object: /
    pattern: "**"
    owner: ec2-user
    group: ec2-user
# hooks: 배포 단계에서 실행될 훅 (hook)을 정의합니다. 
# 여기서는 AfterInstall, ApplicationStart, ValidateService 단계에서 각각 스크립트 파일을 실행합니다. 
# 해당 예시에서는 stop.sh, start.sh, health.sh를 각각 실행한다.
hooks:
  AfterInstall:
    - location: stop.sh # 엔진엑스와 연결되어 있지 않은 스프링 부트를 종료합니다.
      timeout: 60 # 실행 시간 제한(timeout)은 60초로 설정되어 있고
      runas: ec2-user # ec2-user 권한으로 실행합니다.
  ApplicationStart:
    - location: start.sh # 엔진엑스와 연결되어 있지 않은 Port로 새 버전의 스프링 부트를 시작합니다.
      timeout: 60
      runas: ec2-user
  ValidateService:
    - location: health.sh # 새 스프링 부트가 정상적으로 실행됐는지 확인 합니다.
      timeout: 60
      runas: ec2-user
```

### profile.sh

* 공용으로 사용할 'profile'과 포트 체크 로직

```applescript
#!/usr/bin/env bash

# bash는 return value가 안되니 *제일 마지막줄에 echo로 해서 결과 출력*후, 클라이언트에서 값을 사용한다

# 쉬고 있는 profile 찾기: real1이 사용중이면 real2가 쉬고 있고, 반대면 real1이 쉬고 있음
function find_idle_profile()
{
    RESPONSE_CODE=$(curl -s -o /dev/null -w "%{http_code}" http://localhost/profile)

    if [ ${RESPONSE_CODE} -ge 400 ] # 400 보다 크면 (즉, 40x/50x 에러 모두 포함)
    then
        CURRENT_PROFILE=real2
    else
        CURRENT_PROFILE=$(curl -s http://localhost/profile)
    fi

    if [ ${CURRENT_PROFILE} == real1 ]
    then
      IDLE_PROFILE=real2
    else
      IDLE_PROFILE=real1
    fi

    echo "${IDLE_PROFILE}"
}

# 쉬고 있는 profile의 port 찾기
function find_idle_port()
{
    IDLE_PROFILE=$(find_idle_profile)

    if [ ${IDLE_PROFILE} == real1 ]
    then
      echo "8081"
    else
      echo "8082"
    fi
}
```

* `find_idle_profile()`: 현재 사용되지 않는 프로파일을 찾는 함수입니다. 해당 함수는 `http://localhost/profile`로 요청을 보내 응답 코드를 확인하여 현재 사용 중인 프로파일을 판단합니다. 응답 코드가 400 이상인 경우 `real2`를, 그렇지 않은 경우 `http://localhost/profile`로부터 반환된 값을 사용합니다. 현재 사용 중인 프로파일이 `real1`인 경우 `real2`를, 그렇지 않은 경우 `real1`을 쉬고 있는 프로파일로 결정하고 반환합니다.
* `find_idle_port()`: 쉬고 있는 프로파일의 포트를 찾는 함수입니다. `find_idle_profile()` 함수를 호출하여 쉬고 있는 프로파일을 확인한 후, `real1`인 경우 "8081"을, 그렇지 않은 경우 "8082"를 반환합니다.

### stop.sh

* 기존 엔진엑스에 연결되여 있진 않지만, 실행 중이던 스프링 부트 종료한다.

```applescript
#!/usr/bin/env bash

# 현재 스크립트 파일의 절대 경로와 디렉토리 경로를 설정합니다.
ABSPATH=$(readlink -f $0)
ABSDIR=$(dirname $ABSPATH)
# profile.sh 스크립트 파일을 소스하여 해당 스크립트의 내용을 실행환경으로 가져옵니다.
source ${ABSDIR}/profile.sh

# find_idle_port() 함수를 호출하여 쉬고 있는 프로파일의 포트를 확인합니다.
IDLE_PORT=$(find_idle_port)

# 쉬고 있는 프로파일의 포트를 사용하여 구동 중인 애플리케이션의 PID를 확인합니다.
echo "> $IDLE_PORT 에서 구동중인 애플리케이션 pid 확인"
IDLE_PID=$(lsof -ti tcp:${IDLE_PORT})

# 만약 구동 중인 애플리케이션이 없다면 종료하지 않고 메시지를 출력합니다.
if [ -z ${IDLE_PID} ]
then
  echo "> 현재 구동중인 애플리케이션이 없으므로 종료하지 않습니다."
# 구동 중인 애플리케이션이 존재하는 경우 해당 PID에 SIGTERM 시그널을 보내 애플리케이션을 종료합니다.
else
  echo "> kill -15 $IDLE_PID"
  kill -15 ${IDLE_PID}
# 종료 후 5초 동안 대기합니다.
  sleep 5
fi
```

### start.sh

* 배포할 신규 버전 스프링 부트 프로젝트를 stop.sh로 종료한 'profile'로 실행

<pre class="language-applescript"><code class="lang-applescript"># 위의 코드는 Bash 스크립트
<strong>#!/usr/bin/env bash
</strong>
# 현재 스크립트 파일의 절대 경로와 디렉토리 경로를 설정합니다.
ABSPATH=$(readlink -f $0)
ABSDIR=$(dirname $ABSPATH)
# profile.sh 스크립트 파일을 소스하여 해당 스크립트의 내용을 실행환경으로 가져옵니다.
source ${ABSDIR}/profile.sh

# 빌드된 파일을 지정된 디렉토리로 복사합니다.
REPOSITORY=/home/ec2-user/app/step3
PROJECT_NAME=freelec-springboot2-webservice

echo "> Build 파일 복사"
echo "> cp $REPOSITORY/zip/*.jar $REPOSITORY/"

cp $REPOSITORY/zip/*.jar $REPOSITORY/

# 새 애플리케이션 배포: $REPOSITORY/*.jar에서 가장 최신의 JAR 파일을 선택합니다.
echo "> 새 어플리케이션 배포"
JAR_NAME=$(ls -tr $REPOSITORY/*.jar | tail -n 1)

echo "> JAR Name: $JAR_NAME"

# 실행 권한 추가
echo "> $JAR_NAME 에 실행권한 추가"

chmod +x $JAR_NAME

echo "> $JAR_NAME 실행"

# 애플리케이션 실행:
# 프로파일(profile) 정보를 가져옵니다.
IDLE_PROFILE=$(find_idle_profile)

echo "> $JAR_NAME 를 profile=$IDLE_PROFILE 로 실행합니다."

# 이 명령어는 .bashrc 파일을 소스하여 해당 스크립트에서 정의된 환경 변수를 사용할 수 있도록 합니다. 
# 이는 스크립트가 독립적인 환경에서 실행되는 경우에 필요할 수 있습니다.
source ~/.bashrc

# 줄 바꿈 문자: \를 사용하여 스크립트의 한 줄을 여러 줄로 나눌 수 있습니다. 
# \를 줄의 끝에 붙이면 다음 줄로 계속해서 명령어를 작성할 수 있습니다.
nohup java -jar \
    -Dspring.config.location=classpath:/application.properties,classpath:/application-$IDLE_PROFILE.properties,/home/ec2-user/app/application-oauth.properties,/home/ec2-user/app/application-real-db.properties \
    -Dspring.profiles.active=$IDLE_PROFILE \
    $JAR_NAME > $REPOSITORY/nohup.out 2>&#x26;1 &#x26;
</code></pre>

{% hint style="info" %}
`-Dspring.config.location`은 스프링 부트 애플리케이션에서 사용되는 시스템 프로퍼티(System Property)입니다. 이 프로퍼티를 사용하여 스프링 부트 애플리케이션의 구성 파일 위치를 지정할 수 있습니다.



`-Dspring.config.location`을 사용하면 기본적으로 클래스패스(classpath) 상에서 `application.properties` 파일을 찾지만, 추가적인 설정 파일의 위치를 지정할 수 있습니다. 즉, `-Dspring.config.location` 옵션을 사용하여 스프링 부트 애플리케이션에 대한 구성 파일의 경로를 직접 지정할 수 있습니다.

\
예를 들어, `-Dspring.config.location=/home/ec2-user/app/application.properties`라는 옵션을 사용하면 해당 경로에 위치한 `application.properties` 파일을 스프링 부트 애플리케이션의 구성 파일로 사용하게 됩니다. 이를 통해 외부에 위치한 구성 파일을 애플리케이션에 로드할 수 있습니다.
{% endhint %}

{% hint style="info" %}
`-Dspring.profiles.active`는 스프링 부트 애플리케이션에서 사용되는 시스템 프로퍼티(System Property)입니다. 이 프로퍼티를 사용하여 활성화할 프로파일(profile)을 지정할 수 있습니다.



`-Dspring.profiles.active` 옵션을 사용하여 애플리케이션 실행 시 활성화할 프로파일을 지정할 수 있습니다. 예를 들어, `-Dspring.profiles.active=dev`라는 옵션을 사용하면 `dev` 프로파일이 활성화되고, 해당 프로파일에 연결된 프로퍼티 파일의 구성이 적용됩니다.
{% endhint %}

### health.sh

```applescript
#!/usr/bin/env bash

# 변수 설정: $0을 이용하여 현재 스크립트의 절대 경로를 가져오고, 
ABSPATH=$(readlink -f $0)
# dirname 함수를 이용하여 스크립트가 위치한 디렉토리 경로를 추출합니다. 
ABSDIR=$(dirname $ABSPATH)
# 그리고 profile.sh와 switch.sh를 소스로 추가합니다.
source ${ABSDIR}/profile.sh
source ${ABSDIR}/switch.sh

# find_idle_port를 통해 사용 가능한 포트를 가져옵니다.
IDLE_PORT=$(find_idle_port)

# Health Check 시작 메시지 출력 및 대기: 
# "Health Check Start!" 메시지와 IDLE_PORT 값을 출력하고, 10초간 대기합니다.
echo "> Health Check Start!"
echo "> IDLE_PORT: $IDLE_PORT"
echo "> curl -s http://localhost:$IDLE_PORT/profile "
sleep 10

# Health Check 수행: 10회 반복하면서 애플리케이션의 Health Check를 수행합니다. 
for RETRY_COUNT in {1..10}
do
# 각 반복에서는 curl을 사용하여 http://localhost:${IDLE_PORT}/profile에 요청을 보내고
  RESPONSE=$(curl -s http://localhost:${IDLE_PORT}/profile)
  UP_COUNT=$(echo ${RESPONSE} | grep 'real' | wc -l)
  
# 응답을 분석하여 "real"이라는 문자열을 포함하고 있는지 검사합니다.
# Health Check 결과 확인
  if [ ${UP_COUNT} -ge 1 ]
  # 응답에 "real"이 포함되어 있으면 Health Check가 성공한 것으로 간주하고, 
  then # $up_count >= 1 ("real" 문자열이 있는지 검증)
    echo "> Health check 성공"
    # switch_proxy 함수를 호출하여 엔진엑스 프록시를 변경합니다
    switch_proxy
    break
  # 실패한 메시지를 출력합니다.
  else
    echo "> Health check의 응답을 알 수 없거나 혹은 실행 상태가 아닙니다."
    echo "> Health check: ${RESPONSE}"
  fi
  
  # Health Check 실패 시 종료: 10회의 Health Check 시도 중에 실패하면 
  if [ ${RETRY_COUNT} -eq 10 ]
  # "Health check 실패" 메시지를 출력하고 스크립트를 종료합니다.
  then
    echo "> Health check 실패. "
    echo "> 엔진엑스에 연결하지 않고 배포를 종료합니다."
    exit 1
  fi
  
  # Health Check 연결 실패 시 재시도: 
  # Health Check 연결이 실패하면 "Health check 연결 실패. 재시도..." 메시지를 출력하고 
  # 10초 대기한 후 다음 반복을 진행합니다
  echo "> Health check 연결 샐패. 재시도..."
  sleep 10
done

```

### switch.sh

```applescript
#!/usr/bin/env bash

# 변수 설정: $0을 이용하여 현재 스크립트의 절대 경로를 가져오고,
ABSPATH=$(readlink -f $0)
# dirname 함수를 이용하여 스크립트가 위치한 디렉토리 경로를 추출합니다.
ABSDIR=$(dirname $ABSPATH)
# 그리고 profile.sh를 소스로 추가합니다.
source ${ABSDIR}/profile.sh

# 엔진엑스의 프록시 서버 설정을 변경하고 엔진엑스를 다시 로드하는 작업을 수행합니다.
function switch_proxy() {
    # find_idle_port를 통해 사용 가능한 포트를 가져옵니다.
    IDLE_PORT=$(find_idle_port)

    echo "> 전환할 Port: $IDLE_PORT"
    echo "> Port 전환"
    # 프록시 서버 설정 변경: 
    # echo "set \$service_url http://127.0.0.1:${IDLE_PORT};" 명령어를 통해 
    # 엔진엑스의 service-url.inc 파일에 프록시 서버 설정을 변경합니다.
    echo "set \$service_url http://127.0.0.1:${IDLE_PORT};" | sudo tee /etc/nginx/conf.d/service-url.inc

    echo "> 엔진엑스 Reload"
    # 엔진엑스 리로드: sudo service nginx reload 명령어를 통해 엔진엑스를 다시 로드합니다.
    sudo service nginx reload
}
```

{% hint style="info" %}
`tee` 명령어는 파이프로 전달된 입력 데이터를 받아서 동시에 지정된 파일 및 표준 출력으로 출력합니다. 즉, 입력 데이터를 파일에 저장하면서 동시에 화면에 출력할 수 있습니다. 이는 로그 파일 작성이나 출력 내용의 확인 등에 유용하게 사용됩니다.
{% endhint %}

## Refrence

> :books:[스프링 부트와 AWS로 혼자 구현하는 웹 서비스](https://product.kyobobook.co.kr/detail/S000001019679)
