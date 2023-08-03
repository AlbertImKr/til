# Dockerfile

## Docker Format

```docker
# Comment
INSTRUCTION arguments
```

* 명령어는 대소문자를 구분하지 않습니다. 그러나 관례는 인수와 더 쉽게 구별하기 위해 대문자로 지정하는 것입니다.
* Dockerfile는 무조건 **`FROM`**명령어로 시작해야 한다.
* 이것은 **`parser`` ``directives`**, **`comments`**, 와 전역 `ARGs` 뒤에 있을 수 있다.
* **`FROM`** 명령어는 빌딩하려고 하는 **`Parent Image`**를 지정한다
* **`FROM`** 앞에는 Dockerfile 의 FROM 행에 사용하는 인수를 선언하는 하나 이상의 **`ARG`** 명령어만 올 수 있다.
* Docker는 **`#`**으로 시작하는 줄이 **`Parser directives`**이 아니면 **`주석`**으로 처리한다.

```docker
# Comment 
RUN echo 'we are running some # of cool things'
```

## Parser directives

**`Parser directives`**은 선택 사항이며 Dockerfile의 하위 라인이 처리되는 방식에 영향을 준다.

**`Parser directives`**은 **`# directive=value`** 형식의 특수 유형의 주석으로 작성됩니다. 단일 지시문은 한 번만 사용할 수 있습니다.

주석, 빈 줄 또는 빌더 명령이 처리되면 Docker는 더 이상 **`Parser directives`**을 찾지 않습니다.따라서 모든 파서 지시문은 **`Dockerfile`**의 맨 위에 있어야 합니다.

#### Invalid due to appearing twice:

```docker
# directive=value1
# directive=value2

FROM ImageName
```

#### **`Parser directives`**이 아닌 주석 뒤에 있으면 주석으로 처리됩니다.

```docker
# About my dockerfile
# directive=value
FROM ImageName
```

### The following parser directives are supported:

### syntax <a href="#syntax" id="syntax"></a>

이 기능은 **BuildKit** 백엔드를 사용할 때만 사용할 수 있으며 클래식 빌더 백엔드를 사용할 때는 무시됩니다.

* BuildKit은 레거시 빌더를 대체하기 위해 개선된 백엔드입니다. BuildKit은 버전 23.0부터 Docker Desktop 및 Docker Engine 사용자를 위한 기본 빌더입니다.

### escape <a href="#escape" id="escape"></a>

```docker
# escape=\ (backslash)
```

```docker
# escape=` (backtick)
```

**`escape`** 지시문은 Dockerfile에서 문자를 **`escape`**하는 데 사용되는 문자를 설정합니다. 지정하지 않으면 기본 이스케이프 문자는 **\\**입니다.

## Environment replacement <a href="#environment-replacement" id="environment-replacement"></a>

환경 변수(**`ENV`** 문으로 선언됨)는 특정 명령에서 **`Dockerfile`**에서 변수로 사용될 수도 있습니다.

환경 변수는 **`Dockerfile`**에서 **`$variable_name`** 또는 **`${variable_name}`**으로 표시됩니다.

### `${variable_name}` syntax은 아래에 지정된 몇 가지 표준 bash 수정자도 지원합니다.

* **`${variable:-word}`**는 변수가 설정되면 결과가 해당 값이 됨을 나타냅니다. 변수가 설정되지 않으면 단어가 결과가 됩니다.
* **`${variable:+word}`**는 변수가 설정되면 단어가 결과가 되고, 그렇지 않으면 결과는 빈 문자열임을 나타냅니다

### 환경 변수는 Dockerfile의 다음 지시문 목록에서 지원한다

* `ADD`
* `COPY`
* `ENV`
* `EXPOSE`
* `FROM`
* `LABEL`
* `STOPSIGNAL`
* `USER`
* `VOLUME`
* `WORKDIR`
* `ONBUILD` (when combined with one of the supported instructions above)

### .dockerignore <a href="#dockerignore-file" id="dockerignore-file"></a>

docker CLI는 컨텍스트를 docker 데몬으로 보내기 전에 컨텍스트의 루트 디렉터리에서 .dockerignore라는 파일을 찾습니다.

## FROM

```docker
FROM [--platform=<platform>] <image> [AS <name>]
```

Or

```docker
FROM [--platform=<platform>] <image>[:<tag>] [AS <name>]
```

Or

```docker
FROM [--platform=<platform>] <image>[@<digest>] [AS <name>]
```

FROM 명령어는 새 빌드 단계를 초기화하고 후속 명령어에 대한 기본 이미지를 설정합니다.

optional **`--platform`** 플래그는 FROM이 다중 플랫폼 이미지를 참조하는 경우 이미지의 플랫폼을 지정하는 데 사용할 수 있습니다. 예: **`linux/amd64`**, **`linux/arm64`** 또는 **`windows/amd64`**

## RUN

RUN은 2가지 형태

* **`RUN <command>`** (_**`shell`**_ form, the command is run in a shell, which by default is **`/bin/sh -c`** on Linux or **`cmd /S /C`** on Windows)

```docker
RUN /bin/bash -c 'source $HOME/.bashrc && \
echo $HOME'
```

* **`RUN ["executable", "param1", "param2"]`** (_exec_ form)

```docker
RUN /bin/bash -c 'source $HOME/.bashrc && echo $HOME'
```

## RUN --mount

**`RUN --mount`**를 사용하면 빌드가 액세스할 수 있는 파일 시스템 마운트를 만들 수 있습니다. 다음과 같이 사용할 수 있습니다.

* 호스트 파일 시스템 또는 기타 빌드 단계에 바인드 마운트 생성
* 빌드 비밀 또는 ssh-agent 소켓에 액세스
* 영구 패키지 관리 캐시를 사용하여 빌드 속도 향상

### Mount types

<table><thead><tr><th width="228">Type</th><th>Description</th></tr></thead><tbody><tr><td><code>bind</code> (default)</td><td>바인드 마운트 컨텍스트 디렉토리(읽기 전용)</td></tr><tr><td><code>cache</code></td><td>컴파일러 및 패키지 관리자를 위한 캐시 디렉터리에 임시 디렉터리를 마운트합니다.</td></tr><tr><td><code>secret</code></td><td>빌드 컨테이너가 개인 키와 같은 보안 파일을 이미지에 굽지 않고 액세스하도록 허용</td></tr><tr><td><code>ssh</code></td><td>빌드 컨테이너가 암호를 지원하는 SSH 에이전트를 통해 SSH 키에 액세스하도록 허용합니다.</td></tr></tbody></table>

### RUN --network <a href="#run---network" id="run---network"></a>

#### Network types <a href="#network-types" id="network-types"></a>

<table><thead><tr><th width="212">Type</th><th>Description</th></tr></thead><tbody><tr><td><code>default</code> (default)</td><td>기본 네트워크에서 실행</td></tr><tr><td><code>none</code></td><td>네트워크 액세스 없이 실행</td></tr><tr><td><code>host</code></td><td>호스트의 네트워크 환경에서 실행합니다</td></tr></tbody></table>

### CMD <a href="#cmd" id="cmd"></a>

**`CMD`** 명령에는 세 가지 형식이 있습니다

* `CMD ["executable","param1","param2"]` (_exec_ form, this is the preferred form)
* `CMD ["param1","param2"]` (as _default parameters to ENTRYPOINT_)
* `CMD command param1 param2` (_shell_ form)

`CMD`의 주요 목적은 실행 중인 컨테이너에 기본값을 제공하는 것입니다

{% hint style="info" %}
**RUN**은 이미지 **빌드** 단계에서 실행되는 명령어이고, **CMD**는 컨테이너 **실행** 단계에서 실행되는 기본 명령어입니다.
{% endhint %}

## LABEL <a href="#label" id="label"></a>

```docker
LABEL <key>=<value> <key>=<value> <key>=<value> ...
```

LABEL 명령어는 이미지에 메타데이터를 추가합니다. LABEL은 키-값 쌍입니다.

```docker
LABEL "com.example.vendor"="ACME Incorporated"
LABEL com.example.label-with-value="foo"
LABEL version="1.0"
LABEL description="This text illustrates \
that label-values can span multiple lines."
```

## EXPOSE <a href="#expose" id="expose"></a>

```docker
EXPOSE <port> [<port>/<protocol>...]
```

**`EXPOSE`** 명령어는 컨테이너가 런타임 시 지정된 네트워크 **`포트`**에서 수신 대기함을 Docker에 알립니다. 포트가 **`TCP`** 또는 **`UDP`**에서 수신하는지 여부를 지정할 수 있으며 프로토콜이 **`지정되지 않은`** 경우 기본값은 **`TCP`**입니다.

## ENV <a href="#env" id="env"></a>

```docker
ENV <key>=<value> ...
```

**ENV** 명령은 환경 변수를 값으로 설정합니다.&#x20;

## ADD <a href="#add" id="add"></a>

### ADD에는 두 가지 형식

```docker
ADD [--chown=<user>:<group>] [--chmod=<perms>] [--checksum=<checksum>] <src>... <dest>
ADD [--chown=<user>:<group>] [--chmod=<perms>] ["<src>",... "<dest>"]
```

ADD 명령은 **`<src>`**에서 새 파일, 디렉터리 또는 원격 파일 URL을 복사하여 **`<dest>`**경로에 있는 이미지의 파일 시스템에 추가합니다.

## COPY <a href="#copy" id="copy"></a>

```docker
COPY [--chown=<user>:<group>] [--chmod=<perms>] <src>... <dest>
COPY [--chown=<user>:<group>] [--chmod=<perms>] ["<src>",... "<dest>"]
```

## COPY --link <a href="#copy---link" id="copy---link"></a>

COPY 또는 ADD 명령에서 이 플래그를 활성화하면 파일이 자체 레이어에서 독립적으로 유지되고 이전 레이어의 명령이 변경될 때 무효화되지 않는 향상된 의미로 파일을 복사할 수 있습니다.

#### --link 사용의 이점

이전 레이어가 변경된 경우에도 **`--cache-from`**을 사용하여 후속 빌드에서 이미 빌드된 레이어를 재사용하려면 **`--link`**를 사용한다

## ENTRYPOINT <a href="#entrypoint" id="entrypoint"></a>

### ENTRYPOINT 두 가지 형식

The _exec_ form, which is the preferred form:

```docker
ENTRYPOINT ["executable", "param1", "param2"]
```

The _shell_ form:

```docker
ENTRYPOINT command param1 param2
```

**`ENTRYPOINT`**를 사용하면 **`실행 파일`**로 실행될 **`컨테이너`**를 구성할 수 있습니다.

{% hint style="info" %}
CMD 및 ENTRYPOINT 명령은 모두 컨테이너를 실행할 때 실행되는 명령을 정의합니다. 그들의 협력을 설명하는 규칙.

* Dockerfile은 **CMD** 또는 **ENTRYPOINT** 명령 중 하나 이상을 지정해야 합니다.
* 컨테이너를 실행 파일로 사용할 때 **ENTRYPOINT**를 정의해야 합니다.
* **CMD**는 **ENTRYPOINT** 명령에 대한 기본 인수를 정의하거나 컨테이너에서 임시 명령을 실행하기 위한 방법으로 사용해야 합니다.
* **CMD**는 대체 인수로 컨테이너를 실행할 때 재정의됩니다.
{% endhint %}

## VOLUME <a href="#volume" id="volume"></a>

```docker
VOLUME ["/data"]
```

**`VOLUME`** 명령은 지정된 이름으로 마운트 지점을 생성하고 기본 호스트 또는 다른 컨테이너에서 외부 마운트된 볼륨을 보유하는 것으로 표시합니다.

## USER <a href="#user" id="user"></a>

```docker
USER <user>[:<group>]
```

```docker
USER <UID>[:<GID>]
```

**USER** 명령은 사용자 이름(또는 UID) 및 선택적으로 현재 단계의 나머지 부분에 대해 기본 **사용자** 및 **그룹**으로 사용할 사용자 그룹(또는 GID)을 설정합니다.

## WORKDIR <a href="#workdir" id="workdir"></a>

```docker
WORKDIR /path/to/workdir
```

**WORKDIR** 명령어는 Dockerfile에서 뒤에 오는 RUN, CMD, ENTRYPOINT, COPY 및 ADD 명령어에 대한 **작업 디렉터리**를 설정합니다.

## ARG <a href="#arg" id="arg"></a>

```docker
ARG <name>[=<default value>]
```

**ARG** 명령어는 사용자가 **--build-arg** **\<varname>=\<value>** 플래그를 사용하여 **docker build** 명령으로 빌드 시 빌더에 전달할 수 있는 변수를 정의합니다.

## ONBUILD <a href="#onbuild" id="onbuild"></a>

```docker
ONBUILD <INSTRUCTION>
```

ONBUILD 명령은 이미지가 다른 빌드의 기반으로 사용될 때 나중에 실행할 트리거 명령을 이미지에 추가합니다.

작동 방식은 다음과 같습니다

1. **`ONBUILD`** 명령을 만나면 빌더는 빌드 중인 이미지의 메타데이터에 트리거를 추가합니다. 명령은 현재 빌드에 영향을 미치지 않습니다.
2. 빌드가 끝나면 모든 트리거 목록이 이미지 매니페스트의 **`OnBuild`** 키 아래에 저장됩니다. **`docker inspect`** 명령으로 검사할 수 있습니다.
3. 나중에 **`FROM`** 명령을 사용하여 이미지를 새 빌드의 기반으로 사용할 수 있습니다. **`FROM`** 명령 처리의 일부로 다운스트림 빌더는 **`ONBUILD`** 트리거를 찾고 등록된 순서대로 실행합니다. 트리거 중 하나라도 실패하면 **`FROM`** 명령이 중단되어 빌드가 실패합니다. 모든 트리거가 성공하면 **`FROM`** 명령이 완료되고 평소대로 빌드가 계속됩니다.
4. 트리거는 실행 후 최종 이미지에서 지워집니다. 즉, "손자" 빌드에 의해 상속되지 않습니다.

### STOPSIGNAL <a href="#stopsignal" id="stopsignal"></a>

**`STOPSIGNAL`** 명령은 종료하기 위해 컨테이너로 전송될 시스템 호출 신호를 설정합니다.

### HEALTHCHECK <a href="#healthcheck" id="healthcheck"></a>

HEALTHCHECK 명령의 두 가지 형식

* **`HEALTHCHECK [OPTIONS] CMD`** 명령(컨테이너 내부에서 명령을 실행하여 컨테이너 상태 확인)&#x20;
* **`HEALTHCHECK NONE`**(기본 이미지에서 상속된 모든 상태 확인 비활성화)

## SHELL <a href="#shell" id="shell"></a>

```docker
SHELL ["executable", "parameters"]
```

**`SHELL`** 명령을 사용하면 명령의 셸 형식에 사용되는 기본 셸을 재정의할 수 있습니다.&#x20;

## Here-Documents <a href="#here-documents" id="here-documents"></a>

Here-documents는 후속 Dockerfile 행을 **`RUN`** 또는 **`COPY`** 명령의 입력으로 리디렉션하도록 허용합니다.

### Example

```docker
# syntax=docker/dockerfile:1
FROM debian
RUN <<EOT bash
  set -ex
  apt-get update
  apt-get install -y vim
EOT
```

## Reference

{% embed url="https://docs.docker.com/engine/reference/builder/" %}
