# 1 시작하기




## 1.1 버전 관리란?


#### VCS(Version Control System, 버전 관리 시스템)

파일 변화를 시간에 따라 기록했다가 나중에 특정 시점의 버전을 다시 꺼내올 수 있는 시스템



### 1.1.1 로컬 버전 관리

![그림 1-1 로컬 버전 관리](https://raw.githubusercontent.com/esesem/ProGit/master/Figure/1-1.png)



### 1.1.2 중앙집중식 버전 관리(CVCS)

![그림 1-2 중앙집중식 버전 관리(CVCS)](https://raw.githubusercontent.com/esesem/ProGit/master/Figure/1-2.png)

파일을 관리하는 서버가 별도로 있고 클라리언트가 중앙 서버에서 파일을 받아서 사용(Checkout)



### 1.1.3 분산 버전 관리 시스템

![그림 1-3 분산 버전 관리 시스템(DVCS)](https://raw.githubusercontent.com/esesem/ProGit/master/Figure/1-3.png)

클라이언트는 마지막 파일의 스냅샷을 Checkout하지 않고 저장소를 전부 복제한다. 클라이언트 중에서 아무거나 골라도 서버를 복원할 수 있다. 모든 Checkout은 모든 데이터를 가진 진정한 백업이다.  
대부분의 DVCS 환경에서는 리모트 저장소가 존재한다. 그래서 사람들은 동시에 다양한 그룹과 방법으로 협업할 수 있다.




## 1.2 짧게 보는 Git의 역사

리눅스 커널은 규모가 큰 오픈 소스 프로젝트다. 2002년에 리눅스 커널은 BitKeeper라고 불리는 상용 DVCS를 사용하기 시작했다. 2005년에 BitKeeper를 무료로 사용하지 못하게 되어 리눅스 개발 커뮤니티(특히 리누스 토발즈)가 자체 도구를 만드는 계기가 됐다.  

* Git의 초기 목표
  * 빠른 속도
  * 단순한 구조
  * 비선형적인 개발(수천 개의 동시다발적인 브랜치)
  * 완벽한 분산
  * 리눅스 커널 같은 대형 프로젝트에도 유용할 것(속도나 데이터 크기 면에서)

Git은 2005년 탄생하고 나서 아직도 초기 목표를 그대로 유지하고 있다. Git은 동시다발적인 브랜치에도 끄떡없는 슈퍼 울트라 브랜칭 시스템이다.




## 1.3 Git 기초



### 1.3.1 차이가 아니라 스냅샷

![그림 1-4 각 파일에 대한 변화를 저장하는 시스템들](https://raw.githubusercontent.com/esesem/ProGit/master/Figure/1-4.png)

Git은 데이터를 파일 시스템 스냅샷으로 취급하고 크기가 아주 작다. Git은 커밋하거나 프로젝트의 상태를 저장할 때마다 파일이 존재하는 그 순간을 중요하게 여긴다. 파일이 달라지지 않았으면 Git은 성능을 위해서 파일을 새로 저장하지 않는다. 단지 이전 상태의 파일에 대한 링크만 저장한다. Git은 데이터를 **스냅샷의 스트림**처럼 취급한다.

![그림 1-5 시간순으로 프로젝트의 스냅샷을 저장](https://raw.githubusercontent.com/esesem/ProGit/master/Figure/1-5.png)



### 1.3.2 거의 모든 명령을 로컬에서 실행

프로젝트의 모든 히스토리가 로컬 디스크에 있기 때문에 모든 명령을 순식간에 실행한다.



### 1.3.3 Git의 무결성

Git은 데이터를 저장하기 전에 항상 체크섬을 구하고 그 체크섬으로 데이터를 관리한다. 체크섬 없이는 어떠한 파일이나 디렉터리도 변경할 수 없다.  
Git은 SHA-1 해시를 사용하여 체크섬을 만든다.  
Git은 파일을 이름으로 저장하지 않고 해당 파일의 해시로 저장한다.



### 1.3.4 Git은 데이터를 추가할 뿐

Git으로 무얼 하든 Git 데이터베이스에 데이터가 추가된다. 다른 VCS처럼 Git도 커밋하지 않으면 변경사항을 잃어버릴 수 있다. 하지만 일단 스냅샷을 커밋하고 나면 데이터를 잃어버리기 어렵다.



### 1.3.5 세 가지 상태


#### Git은 파일을 Committed, Modified, Staged 세 가지 상태로 관리

* Committed: 데이터가 로컬 데이터베이스에 안전하게 저장됨
* Modified: 수정한 파일을 아직 로컬 데이터베이스에 커밋하지 않음
* Staged: 현재 수정한 파일을 곧 커밋할 것이라고 표시한 상태

![그림 1-6 워킹 디렉터리, Staging Area, Git 디렉터리](https://raw.githubusercontent.com/esesem/ProGit/master/Figure/1-6.png)

* Git 디렉터리: Git이 프로젝트의 메타데이터와 객체 데이터베이스를 저장하는 곳, 다른 컴퓨터에 있는 저장소를 Clone할 때 Git 디렉터리가 만들어짐
* 워킹 디렉터리: 프로젝트의 특정 버전을 Checkout한 것, Git 디렉터리는 지금 작업하는 디스크에 있고 그 디렉터리 안에 압축된 데이터베이스에서 파일을 가져와서 워킹 디렉터리를 만듦
* Staging Area: Git 디렉터리에 있음, 단순한 파일이고 곧 커밋할 파일에 대한 정보를 저장


#### Git으로 하는 일

1. 워킹 디렉터리에서 파일을 수정한다.
2. Staging Area에 파일을 Stage해서 커밋할 스냅샷을 만든다.
3. Staging Area에 있는 파일들을 커밋해서 Git 디렉터리에 영구적인 스냅샷으로 저장한다.

* Git 디렉터리에 있는 파일들은 Committed 상태
* 파일을 수정하고 Staging Area에 추가했다면 Staged
* Checkout하고 나서 수정했지만, Staging Area에 추가하지 않았으면 Modified




## 1.4 CLI

Git을 사용하는 방법은 CLI(Command Line Interface)로 사용할 수도 있고 GUI를 사용할 수도 있다. Git의 모든 기능을 지원하는 것은 CLI 뿐이다.




## 1.5 Git 설치



### 1.5.1 리눅스에 설치

```shell
$ sudo yum install git
$ sudo apt-get install git
```

* [http://git-scm.com/download/linux](http://git-scm.com/download/linux)



### 1.5.2 맥에 설치

* Xcode Command Line Tools
* [http://git-scm.com/download/mac](http://git-scm.com/download/mac)
* [http://mac.github.com](http://mac.github.com)



### 1.5.3 윈도우에 설치

* [http://git-scm.com/download/win](http://git-scm.com/download/win)
* [https://git-for-windows.github.io](https://git-for-windows.github.io)
* [http://windows.github.com](http://windows.github.com)



### 1.5.4 소스 코드로 설치하기


#### 필요한 라이브러리

* curl
* zlib
* openssl
* expat


#### 패키지 설치

```shell
$ sudo yum install curl-devel gettext-devel \
  openssl-devel perl-devel zlib-devel
$ sudo apt-get install libcurl4-gnutls-dev libexpat1-dev gettext \
  libz-dev libssl-dev
```


#### doc, html, info 형식의 문서를 이용하려면 아래의 의존 패키지 필요

```shell
$ sudo yum install asciidoc xmlto docbook2X
$ sudo apt-get install asciidoc xmlto docbook2x
```


#### 컴파일하고 설치

```shell
$ tar -zxf git-2.0.0.tar.gz
$ cd git-2.0.0
$ make configure
$ ./configure --prefix=/usr
$ make all doc info
$ sudo make install install-doc install-html install-info
```




## 1.6 Git 최초 설정


#### git config 도구

각 설정은 역순으로 우선시 됨

1. `/etc/gitconfig`: 시스템의 모든 사용자와 모든 저장소에 적용되는 설정, `git config --system` 옵션으로 이 파일을 읽고 쓸 수 있음
2. `~/.gitconfig`, `~/.config/git/config`: 특정 사용자에게만 적용되는 설정, `git config --global` 옵션으로 이 파일을 읽고 쓸 수 있음
3. `.git/config`: Git 디렉터리에 있고 특정 저장소(혹은 현재 작업중인 프로젝트)에만 적용



### 1.6.1 사용자 정보


#### 사용자 이름과 이메일 주소 설정

프로젝트마다 다른 이름과 이메일 주소를 사용하고 싶으면 `--global` 옵션을 빼고 실행
```shell
$ git config --global user.name "John Doe"
$ git config --global user.email "johndoe@example.com"
```



### 1.6.2 편집기

```shell
$ git config --global core.editor emacs
```



### 1.6.3 설정 확인

```shell
$ git config --list
user.name=John Doe
user.email=johndoe@example.com
color.status=auto
color.branch=auto
color.interactive=auto
color.diff=auto
...
```


#### 특정 Key에 대해 어떤 값을 사용하는지 확인

```shell
$ git config user.name
John Doe
```




## 1.7 도움말 보기

```shell
$ git help <verb>
$ git <verb> --help
$ man git-<verb>
```