## 1.1 버전 관리란?

> VCS(Version Control System, 버전 관리 시스템)는 파일 변화를 시간에 따라 기록했다가 나중에 특정 시점의 버전을 다시 꺼내올 수 있는 시스템이다. VCS를 사용하면 각 파일을 이전 상태로 되돌릴 수 있고, 프로젝트를 통째로 이전 상태로 되돌릴 수 있고, 시간에 따라 수정 내용을 비교해 볼 수 있고, 누가 문제를 일으켰는지도 추적할 수 있고, 누가 언제 만들어낸 이슈인지도 알 수 있다. 파일을 잃어버리거나 잘못 거쳤을 때도 쉽게 복구할 수 있다.



### 로컬 버전 관리

로컬 VCS는 간단한 데이터페이스를 사용해서 파일의 변경 정보를 관리한다.

![그림 1-1 로컬 버전 관리](https://raw.githubusercontent.com/esesem/ProGit/master/Figure/1-1.png)

많이 쓰이는 VCS 도구 중에 RCS(Revision Control System)라는 것이 있다. RCS는 기본적으로 Patch Set(파일에서 변경되는 부분)을 관리한다. 이 Patch Set은 특별한 형식의 파일로 저장한다. 그리고 일련의 Patch Set을 적용해서 모든 파일을 특정 시점으로 되돌릴 수 있다.



### 중앙집중식 버전 관리(CVCS)

프로젝트를 진행하다 보면 다른 개발자와 함께 작업해야 하는 경우가 많다. 이럴 때 생기는 문제를 해결하기 위해 CVCS(중앙집중식 VCS)가 개발됐다. CVS, Subversion, Perforce 같은 시스템은 파일을 관리하는 서버가 별도로 있고 클라이언트가 중앙 서버에서 파일을 받아서 사용(Checkout)한다.

![그림 1-2 중앙집중식 버전 관리(CVCS)](https://raw.githubusercontent.com/esesem/ProGit/master/Figure/1-2.png)

CVCS 환경은 로컬 VCS에 비해 장점이 많다.

* 모두 누가 무엇을 하고 있는지 알 수 있다.
* 관리자는 누가 무엇을 할지 꼼꼼하게 관리할 수 있다.
* 모든 클라이언트의 로컬 데이터베이스를 관리하는 것보다 VCS 하나를 관리하기가 훨씬 쉽다.
그러나 몇 가지 치명적인 결점이 있다.
* 중앙 서버에 발생한 문제로 만약 서버가 한 시간 동안 다운되면 그동안 아무도 다른 사람과 협업할 수 없고 하던 일을 백업할 방법도 없다.
* 중앙 데이터베이스가 있는 하드디스크에 문제가 생기면 프로젝트의 모든 히스토리를 잃는다. 물론 사람마다 하나씩 가진 스냅샷은 괜찮다. 로컬 VCS도 이와 비슷한 결점이 있고 이런 문제가 발생하면 모든 것을 잃는다.



### 분산 버전 관리 시스템(DVCS)

Git, Mecurial, Bazaar, Darcs 같은 DVCS에서의 클라이언트는 단순히 파일의 마지막 스냅샷을 Checkout하지 않는다. <u>저장소를 전부 복제한다.</u> 서버에 문제가 생기면 이 복제물로 다시 작업을 시작할 수 있다. 클라이언트 중에서 아무거나 골라도 서버를 복원할 수 있다. 모든 Checkout은 모든 데이터를 가진 진정한 백업이다.

![그림 1-3 분산 버전 관리 시스템(DVCS)](https://raw.githubusercontent.com/esesem/ProGit/master/Figure/1-3.png)

대부분의 DVCS 환경에서는 <u>리모트 저장소</u>가 존재한다. 리모트 저장소가 많을 수도 있다. 그래서 사람들은 동시에 다양한 그룹과 다양한 방법으로 협업할 수 있다. 계층 모델 같은 중앙집중식 시스템으로는 할 수 없는 워크플로를 다양하게 사용할 수 있다.




## 1.2 짧게 보는 Git의 역사

리눅스(Linux) 커널은 굉장히 규모가 큰 오픈 소스 프로젝트다. 리눅스 커널의 삶 대부분은(1991-2002) Patch와 단순 압축 파일로만 관리했다. 2002년에 드디어 리눅스 커널은 BitKeeper라고 불리는 상용 DVCS를 사용하기 시작했다.

2005년에 커뮤니티가 만드는 리눅스 커널과 이익을 추구하는 회사가 개발한 BitKeeper의 관계가 틀어졌다. BitKeeper를 무료로 사용하지 못하게 된 것이다. 이 사건은 리눅스 개발 커뮤니티(특히 리눅스 창시자 리누스 토발즈)가 자체 도구를 만드는 계기가 됐다. Git은 BitKeeper를 사용하면서 배운 교훈을 기초로 아래와 같은 목표를 세웠다.

* 빠른 속도
* 단순한 구조
* 비선형적인 개발(수천 개의 동시다발적인 브랜치)
* 완벽한 분산
* 리눅스 커널 같은 대형 프로젝트에도 유용할 것(속도나 데이터 크기 면에서)

Git은 2005년 탄생하고 나서 아직도 초기 목표를 그대로 유지하고 있다. 그러면서도 사용하기 쉽게 진화하고 성숙했다. Git은 미친 듯이 빨라서 대형 프로젝트에 사용하기도 좋다. Git은 동시다발적인 브랜치에도 끄떡없는 슈퍼 울트라 브랜칭 시스템이다.




## 1.3 Git 기초



### 차이가 아니라 스냅샷

Subversion과 Subversion 비슷한 VCS들과 Git의 가장 큰 차이점은 <u>데이터를 다루는 방법</u>에 있다. 큰 틀에서 봤을 때 VCS 대부분은 관리하는 정보가 파일들의 목록이다. CVS, Subversion, Perforce, Bazaar 등의 시스템은 각 파일의 변화를 시간순으로 관리하면서 파일들의 집합을 관리한다.

![그림 1-4 각 파일에 대한 변화를 저장하는 시스템들](https://raw.githubusercontent.com/esesem/ProGit/master/Figure/1-4.png)

Git은 데이터를 파일 시스템 스냅샷으로 취급하고 크기가 아주 작다. Git은 커밋하거나 프로젝트의 상태를 저장할 때마다 파일이 존재하는 그 순간을 중요하게 여긴다. 파일이 달라지지 않았으면 Git은 성능을 위해서 파일을 새로 저장하지 않는다. 단지 이전 상태의 파일에 대한 링크만 저장한다. Git은 데이터를 **스냅샷의 스트림**처럼 취급한다.

![그림 1-5 시간순으로 프로젝트의 스냅샷을 저장](https://raw.githubusercontent.com/esesem/ProGit/master/Figure/1-5.png)

> Git은 강력한 도구를 지원하는 작은 파일 시스템이다. Git은 단순한 VCS가 아니다.



### 거의 모든 명령을 로컬에서 실행

거의 모든 명령이 로컬 파일과 데이터만 사용하기 때문에 네트워크에 있는 다른 컴퓨터는 필요 없다. 프로젝트의 모든 히스토리가 로컬 디스크에 있기 때문에 모든 명령을 순식간에 실행한다.

예를 들어 Git은 프로젝트의 히스토리를 조회할 때 서버 없이 조회한다. 로컬 데이터베이스에서 히스토리를 읽어서 보여 준다. 어떤 파일의 현재 버전과 한 달 전의 상태를 보고 싶을 때도 한 달 전의 파일과 지금의 파일을 로컬에서 찾는다. 리모트에 있는 서버에 접근하고 나서 예전 버전을 가져올 필요가 없다.

즉, 오프라인 상태이거나 VPN(virtual private network)으로 연결할 수 없어도 막힘없이 일할 수 있다. 다른 VCS에서는 불가능한 일이다. Perforce를 예로 들자면 서버에 연결할 수 없을 때 할 수 있는 일이 별로 없다. 오프라인이기 때문에 데이터베이스에 접근할 수 없어서 파일을 편집할 수는 있지만, 커밋할 수 없다.



### Git의 무결성

<u>Git은 데이터를 저장하기 전에 항상 체크섬을 구하고 그 체크섬으로 데이터를 관리한다.</u> 그래서 체크섬 없이는 어떠한 파일이나 디렉터리도 변경할 수 없다.

> 체크섬은 Git에서 사용하는 가장  기본적인(atomic) 데이터 단위이자 Git의 기본 철학이다. Git 없이는 체크섬을 다룰 수 없어서 파일의 상태도 알 수 없고 심지어 데이터를 잃어버릴 수도 있다.

Git은 SHA-1 해시를 사용하여 체크섬을 만든다. 만든 체크섬은 40자 길이의 16진수 문자열이다. 파일의 내용이나 디렉터리 구조를 이용하여 체크섬을 구한다.

```
24b9da6552252987aa493b52f8696cd6d3b00373
```

> Git은 파일을 이름으로 저장하지 않고 해당 파일의 해시로 저장한다.



### Git은 데이터를 추가할 뿐

Git으로 무얼 하든 Git 데이터베이스에 데이터가 추가된다. 다른 VCS처럼 Git도 커밋하지 않으면 변경사항을 잃어버릴 수 있다. 하지만 일단 스냅샷을 커밋하고 나면 데이터를 잃어버리기 어렵다.



### 세 가지 상태

> Git은 파일을 Committed, Modified, Staged 이렇게 세 가지 상태로 관리한다.
> * Committed: 데이터가 로컬 데이터베이스에 안전하게 저장됨
> * Modified: 수정한 파일을 아직 로컬 데이터베이스에 커밋하지 않음
> * Staged: 현재 수정한 파일을 곧 커밋할 것이라고 표시

이 세 가지 상태는 Git 프로젝트의 세 가지 단계와 연결돼 있다.

![그림 1-6 워킹 디렉터리, Staging Area, Git 디렉터리](https://raw.githubusercontent.com/esesem/ProGit/master/Figure/1-6.png)

* Git 디렉터리는 Git이 프로젝트의 메타데이터와 객체 데이터베이스를 저장하는 곳을 말한다. 다른 컴퓨터에 있는 저장소를 Clone할 때 Git 디렉터리가 만들어진다.
* 워킹 디렉터리는 프로젝트의 특정 버전을 Checkout한 것이다. Git 디렉터리는 지금 작업하는 디스크에 있고 그 디렉터리 안에 압축된 데이터베이스에서 파일을 가져와서 워킹 디렉터리를 만든다.
* Staging Area는 Git 디렉터리에 있다. 단순한 파일이고 곧 커밋할 파일에 대한 정보를 저장한다.

> Git으로 하는 일은 기본적으로 아래와 같다.
> 1. 워킹 디렉터리에서 파일을 수정한다.
> 2. Staging Area에 파일을 Stage해서 커밋할 스냅샷을 만든다.
> 3. Staging Area에 있는 파일들을 커밋해서 Git 디렉터리에 영구적인 스냅샷으로 저장한다.

Git 디렉터리에 있는 파일들은 Committed 상태이다. 파일을 수정하고 Staging Area에 추가했다면 Staged이다. 그리고 Checkout하고 나서 수정했지만, 아직 Staging Area에 추가하지 않았으면 Modified이다.




## 1.4 CLI

Git을 CLI(Command Line Interface)로 사용할 수도 있고 GUI를 사용할 수도 있다. Git의 모든 기능을 지원하는 것은 CLI 뿐이다.




## 1.5 Git 설치



### 리눅스에 설치

```shell
$ sudo yum install git
$ sudo apt-get install git
```



### 맥에 설치

Xcode Command Line Tools가 설치하기 가장 쉽다.

* [OSX용 Git 인스톨러](http://git-scm.com/download/mac)
* [GitHub for Mac](http://mac.github.com)



### 윈도우에 설치

* [공식 배포판(Git for Windows)](http://git-scm.com/download/win)
* [GitHub for Windows](http://windows.github.com)



### 소스 코드로 설치하기

Git은 curl, zlib, openssl, expat, libiconv를 필요로 한다.

```shell
$ sudo yum install curl-devel expat-devel gettext-devel \
  openssl-devel perl-devel zlib-devel
$ sudo apt-get install libcurl4-gnutls-dev libexpat1-dev gettext \
  libz-dev libssl-dev
```

다양한(doc, html, info) 형식의 문서를 이용하려면 아래의 의존 패키지가 필요하다.

```shell
$ sudo yum install asciidoc xmlto docbook2X
$ sudo apt-get install asciidoc xmlto docbook2x
```

Fedora/RHEL/RHEL류 사용자라면 아래 명령도 필요하다(바이너리 이름이 달라서 그렇다).

```shell
$ sudo ln -s /usr/bin/db2x_docbook2texi /usr/bin/docbook2x-texi
```

모든 게 준비되면 바로 최신 배포 버전을 가져온다. [Kernel.org](https://www.kernel.org/pub/software/scm/git)에서 내려받을 수도 있고 [GitHub 미러](https://github.com/git/git/releases)에서 받을 수도 있다. 보통 GitHub 페이지에서 최신 버전을 내려받는 것이 더 간단하지만 kernel.org에는 배포 서명이 있어서 내려받은 것을 검증할 수 있다.

```shell
$ tar -zxf git-2.0.0.tar.gz
$ cd git-2.0.0
$ make configure
$ ./configure --prefix=/usr
$ make all doc info
$ sudo make install install-doc install-html install-info
```

설치하고 나면 Git을 사용하여 Git 소스 코드를 수정할 수도 있다.

```git
$ git clone git://git.kernel.org/pub/scm/git/git.git
```




## 1.6 Git 최초 설정

git config라는 도구로 설정 내용을 확인하고 변경할 수 있다.

1. `/etc/gitconfig` 파일: 시스템의 모든 사용자와 모든 저장소에 적용되는 설정이다. `git config --system` 옵션으로 이 파일을 읽고 쓸 수 있다.
2. `~/.gitconfig`, `~/.config/git/config` 파일: 특정 사용자에게만 적용되는 설정이다. `git config --global` 옵션으로 이 파일을 읽고 쓸 수 있다.
3. `.git/config` 파일: Git 디렉터리에 있고 특정 저장소(혹은 현재 작업 중인 프로젝트)에만 적용된다.

각 설정은 역순으로 우선시 된다.

`.gitconfig` 파일

* 윈도우: `C:\Users\$USER`
* 윈도우 XP: `C:\Documents and Settings\All Users\Application Data\Git\config`
* 윈도우 Vista 이후 버전: `C:\ProgramData\Git\config`

시스템 설정 파일의 경로는 `git config -f <file>` 명령으로 변경할 수 있다.



### 사용자 정보

사용자 이름과 이메일 주소를 설정

```git
$ git config --global user.name "John Doe"
$ git config --global user.email johndoe@example.com
```

`--global` 옵션으로 설정하는 것은 딱 한 번만 하면 된다. 프로젝트마다 다른 이름과 이메일 주소를 사용하고 싶으면 `--global` 옵션을 빼고 명령을 실행한다.



### 편집기

```git
$ git config --global core.editor emacs
```



### 설정 확인

```git
$ git config --list
user.name=John Doe
user.email=johndoe@example.com
color.status=auto
color.branch=auto
color.interactive=auto
color.diff=auto
...
```

`git config <key>` 명령으로 특정 Key에 대해 어떤 값을 사용하는지 확인할 수 있다.

```git
$ git config user.name
John Doe
```



## 1.7 도움말 보기

```git
$ git help <verb>
$ git <verb> --help
$ man git-<verb>
```

* [Freenode IRC 서버](https://freenode.net): #git이나 #github 채널




## 1.8 요약

* Git이 무엇이고 지금까지 사용해 온 다른 CVCS와 어떻게 다른지
* 시스템에 Git을 설치하고 사용자 정보 설정