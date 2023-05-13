# AWS EC2

## 인스턴스&#x20;

* Amazon Linux AMI(Amazon Machine Image)(리눅스 1)&#x20;
  * 아마존이 개발하고 있기 때문에 지원받기가 쉽다.
  * 레드햇 베이스이므로 레드햇 계열의 배포판을 많이 다뤄본 사람일수록 문제없이 사용할 수 있다.
  * AWS의 각종 서비스와의 상성이 좋다
  * Amazon 독자적인 개발 리포지터리를 사용하고 있어 yum이 매우 빠르다.

## 보안그룹

### SSH

* PORT 22
  * AWS EC2에 터미널로 접속
    * [pem 키](pem-privacy-enhanced-mail.md)가 있어야 접속할 수 있지만 지정된 IP에서만 ssh 접속이 가능하도록 하는 것이 보안이 좋다.

## EIP

* AWS의 고정 IP를 Elastic IP(EIP, 탄력적 IP)라고 합니다.

{% hint style="info" %}
탄력적 IP는 생성하고 EC2 서번에 연결하지 않으면 비용이 발생합니다.  즉, 생성된 탄력적 IP는 무조건 EC2에 바로 연결해야 한다.
{% endhint %}

## EC2 서버에 접속하기

### 명령어로 연결

```bash
ssh - i ${pem 키 위치} ${EC2의 탈력적 IP 주소}
```

### SSH CONFIG 등록

1. pem 파일을 `~/.ssh/` 로 복사한다. `~/.ssh/` 디렉토리로 pem 파일을 옮겨 놓으면 `ssh` 실행 시 **pem 키 파일을 자동으로 읽어** 접속 진행합니다.&#x20;

{% code lineNumbers="true" %}
```bash
cp ${pem 키를 내려받은 위치} ~/.ssh/
cd ~/.ssh/ # pem 키 파일이 있는 디렉토리로 이동
chmod 600 ~/.ssh/${pem키이름} # 권한 설정
```
{% endcode %}

2. config 파일에 등록한다.

```bash
vim ~/.ssh/config  # config 파일을 생성 혹은 편집한다.
```

```java
# 주석
Host ${본인이 원하는 서비스명}
    HostName ${ec2의 탄력적 IP 주소}
    User ec2-user
    IndentityFile ~/.ssh/${pem키 이름}
```

{% hint style="info" %}
ssh\_config 파일은 설정 항목과 해당 값 사이에 공백 또는 탭 문자를 사용하여 구성되어 있습니다. 각 줄은 설정 항목과 해당 값을 포함하며, 주석은 "#" 기호로 시작합니다. 설정 항목과 값은 콜론(:), 등호(=) 또는 공백 문자로 구분될 수 있습니다.
{% endhint %}

<pre class="language-java"><code class="lang-java"><strong># 예시
</strong><strong>Host example.com
</strong>    Port 22
    User myusername
    IdentityFile ~/.ssh/id_rsa

Host myserver
    HostName 192.168.0.100
    Port 2222
    User admin
</code></pre>

3. 실행 관한이 필요합니다.

```bash
chmod 700 ~/.ssh/config # 실행 권한 설정
```

4. 실행

```bash
ssh ${config에 등록한 서비스명}
```

## 아마존 리눅스 1 서버 생성 시 꼭 해야 할 설정들

### 1. Java 설치

### 2. 타임존 변경

<pre class="language-bash" data-line-numbers><code class="lang-bash">#Linux 시스템에서 현재 로컬 시간대 정보를 삭제하는 명령어
sudo rm /etc/localtime 
<strong>
</strong><strong># 이 명령어를 실행하면 시스템의 로컬 시간대가 서울 시간대로 변경됩니다
</strong>sudo ln -s /user/share/zoneinfo/Asia/Seoul /etc/localtime 
</code></pre>

{% embed url="https://www.baeldung.com/linux/change-timezone" %}

### 3. 호스트네임 변경

* 이유: 여러 서버를 관리 중일 경우 IP만으로 어떤 서비스의 서버인지 확인이 어렵다.

1. Hostname 등록

{% code lineNumbers="true" %}
```bash
# network 파일 편집
sudo vim /etc/sysconfig/network

# network 파일 중 HOSTNAME이름을 변경

# 서버를 재부팅
sudo reboot 
```
{% endcode %}

2. `/ect/hosts`에 Hostname등록

{% code lineNumbers="true" %}
```bash
# hosts 파일 편집
sudo vim /etc/hosts

# 파일에 해당 내용 추가
# 127.0.0.1 등록한 HOSTNAME

# 등록 테스트
curl 등록한 호스트 이름
```
{% endcode %}

## Refrence

> :books:[스프링 부트와 AWS로 혼자 구현하는 웹 서비스](https://product.kyobobook.co.kr/detail/S000001019679)
