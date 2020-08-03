시작하기
========

<br>

# 1.1 버전 관리란?

### VCS(Version Control System, 버전 관리 시스템)

파일 변화를 시간에 따라 기록했다가 나중에 특정 시점의 버전을 다시 꺼내올 수 있는 시스템

* 각 파일을 이전 상태로 되돌릴 수 있다.
* 시간에 따라 수정 내용을 비교해 볼 수 있다.
* 누가 문제를 일으켰는지도 추적할 수 있다.
* 누가 언제 만들어낸 이슈인지도 알 수 있다.
* 파일을 잃어버리거나 잘못 고쳤을 때도 쉽게 복구 할 수 있다.
* 이런 모든 장점을 큰 노력 없이 이용할 수 있다.

<br>

## 로컬 버전 관리

간단한 버전 관리: 디렉터리로 파일을 복사하는 방법

* 디렉터리 이름에 시간을 넣는다.
* 간단하므로 자주 사용
* 정말 뭔가 잘못되기 쉽다.
* 작업하던 디렉터리를 지워버리거나, 실수로 파일을 잘못 고칠 수도 있고, 잘못 복사할 수도 있다.

### 로컬 VCS

![그림 1-1 로컬 버전 관리](https://raw.githubusercontent.com/esesem/ProGit/master/Figure/1-1.png)

* 아주 간단한 데이터베이스를 사용해서 파일의 변경 정보를 관리
* RCS(Revision Control System) 등

<br>

## 중앙집중식 버전 관리(CVCS)

![그림 1-2 중앙집중식 버전 관리(CVCS)](https://raw.githubusercontent.com/esesem/ProGit/master/Figure/1-2.png)

* 다른 개발자와 함께 작업해야 하는 경우
* VCS, Subversion, Perforce 등
* 파일을 관리하는 서버가 별도로 있고 클라이언트가 중앙 서버에서 파일을 받아서 사용(Checkout)
* 모두 누구가 무엇을 하고 있는지 알 수 있다.
* 관리자는 누가 무엇을 할지 꼼꼼하게 관리할 수 있다.
* 모든 클라이언트의 로컬 데이터베이스를 관리하는 것보다 VCS 하나를 관리하기가 훨씬 쉽다.

### 치명적인 결점

* 서버가 다운되면 그동안 아무도 다른 사람과 협업할 수 없고 하던 일을 백업할 방법도 없다.
* 중앙 데이터 베이스가 있는 하드디스크에 문제가 생기면 프로젝트의 모든 히스토리를 잃는다.
* 로컬 VCS도 이와 비슷한 결점이 있고 이런 문제가 발생하면 모든 것을 잃는다.

<br>

## 분산 버전 관리 시스템

### DVCS

![그림 1-3 분산 버전 관리 시스템(DVCS)](https://raw.githubusercontent.com/esesem/ProGit/master/Figure/1-3.png)

* Git, Mecurial, Bazaar, Darcs 등
* 클라이언트는 단순히 파일의 마지막 스냅샷을 Checkout하지 않는다.
* 그냥 저장소를 전부 복제한다.
* 서버에 문제가 생기면 이 복제물로 다시 작업을 시작할 수 있다.
* 클라이언트 중에서 아무거나 골라도 서버를 복원할 수 있다.
* 모든 Checkout은 모든 데이터를 가진 진정한 백업이다.
* 대부분의 DVCS 환경에서는 **리모트 저장소**가 존재한다. 많을 수도 있다.
* 사람들은 동시에 다양한 그룹과 다양한 방법으로 협업할 수 있다.

<br>

# 1.2 짧게 보는 Git의 역사

* 리눅스 커널은(1991-2002) Patch와 단순 압축 파일로만 관리
* 2002년 드디어 BitKeeper라고 불리는 상용 DVCS를 사용하기 시작
* 2005년 BitKeeper를 무료로 사용하지 못하게 됨
* Git 탄생
  * 빠른 속도
  * 단순한 구조
  * 비선형적인 개발(수천 개의 동시다발적인 브랜치)
  * 완벽한 분산
  * 리눅스 커널 같은 대형 프로젝트에도 유용할 것(속도나 데이터 크기 면에서)
* Git은 동시다발적인 브랜치에도 끄떡없는 슈퍼 울트라 브랜칭 시스템이다.

<br>

# 1.3 Git 기초

## 차이가 아니라 스냅샷

### CVS, Subversion, Perforce, Bazaar 등

![그림 1-4 각 파일에 대한 변화를 저장하는 시스템들](https://raw.githubusercontent.com/esesem/ProGit/master/Figure/1-4.png)

* 시스템은 각 파일의 변화를 시간순으로 관리하면서 파일들의 집합을 관리한다.

### Git

![그림 1-5 시간순으로 프로젝트의 스냅샷을 저장](https://raw.githubusercontent.com/esesem/ProGit/master/Figure/1-5.png)

* 데이터를 파일 시스템 스냅샷으로 취급하고 크기가 아주 작다.
* 커밋하거나 프로젝트의 상태를 저장할 때마다 파일이 존재하는 그 순간을 중요하게 여긴다.
* 파일이 달라지지 않았으면 성능을 위해서 파일을 새로 저장하지 않는다.
* 단지 이전 상태의 파일에 대한 링크만 저장한다.
* 데이터를 **스냅샷의 스트림**처럼 취급한다.

<br>

## 거의 모든 명령을 로컬에서 실행

* 거의 모든 명령이 로컬 파일과 데이터만 사용
* 프로젝트의 모든 히스토리가 로컬 디스크에 있기 때문에 모든 명령을 순식간에 실행한다.
* 히스토리를 조회할 때 서버 없이 조회한다.
* 파일을 비교하기 위해 리모트에 있는 서버에 접근하고 나서 예전 버전을 가져올 필요가 없다.
* 오프라인 상태이거나 VPN(virtual private network)으로 연결할 수 없어도 막힘없이 일할 수 있다.
* 오프라인이기 때문에 데이터베이스에 접근할 수 없어서 파일을 편집할 수는 없지만, 커밋할 수 없다.

<br>

## Git의 무결성

### 체크섬

가장 기본적인(atomic) 데이터 단위이자 Git의 기본 철학이다.

* Git은 데이터를 저장하기 전에 항상 체크섬을 구하고 그 체크섬으로 데이터를 관리한다.
* 체크섬 없이는 어떠한 파일이나 디렉터리도 변경할 수 없다.
* Git 없이는 체크섬을 다룰 수 없어서 파일의 상태도 알 수 없고 심지어 데이터를 잃어버릴 수 있다.
* Git은 SHA-1 해시를 사용하여 해시를 만든다.
* 40자 길이의 16진수 문자열
    ```
    24b9da6552252987aa493b52f8696cd6d3b00373
    ```
* Git은 파일을 이름으로 저장하지 않고 해당 파일의 해시로 저장한다.

<br>

## Git은 데이터를 추가할 뿐

* Git으로 무얼 하든 Git 데이터베이스에 데이터가 추가된다.
* 되돌리거나 데이터를 삭제할 방법이 없다.
* Git도 커밋하지 않으면 변경사항을 잃어버릴 수 있다.
* 일단 스냅샷을 커밋하고 나면 데이터를 잃어버리기 어렵다.

<br>

## 세 가지 상태

![그림 1-6 워킹 디렉터리, Staging Area, Git 디렉터리](https://raw.githubusercontent.com/esesem/ProGit/master/Figure/1-6.png)

### 파일 관리

* **Committed**: 데이터가 로컬 데이터베이스에 안전하게 저장됐다는 것을 의미
  * Git 디렉터리에 있는 파일들은 Committed 상태이다.
* **Modified**: 수정한 파일을 아직 로컬 데이터베이스에 커밋하지 않은 것을 의미
  * Checkout하고 나서 수정했지만, 아직 Staging Area에 추가하지 않았으면 Modified이다.
* **Staged**: 현재 수정한 파일을 곧 커밋할 것이라고 표시한 상태를 의미
  * 파일을 수정하고 Staging Area에 추가했다면 Staged이다.

### Git 프로젝트의 세 가지 단계

* **Git 디렉터리**: Git이 프로젝트의 메타데이터와 객체 데이터베이스를 저장하는 곳
  * 다른 컴퓨터에 있는 저장소를 Clone할 때 만들어진다.
  * 지금 작업하는 디스크에 있다.
* **워킹 디렉터리**: 프로젝트의 특정 버전을 checkout한 것
  * Git 디렉터리 안에 압축된 데이터베이스에서 파일을 가져와서 워킹 디렉터리를 만든다.
* **Staging Area**: 단순한 파일이고 곧 커밋할 파일에 대한 정보를 저장
  * Git 디렉터리에 있다.
  * 종종 인덱스라고 불리기도 한다.

### Git으로 하는 일

1. 워킹 디렉터리에서 파일을 수정한다.
2. Staging Area에 파일을 Stage해서 커밋할 스냅샷을 만든다.
3. Staging Area에 있는 파일들을 커밋해서 Git 디렉터리에 영구적인 스냅샷으로 저장한다.

<br>

# 1.4 CLI

### CLI(Command Line Interface)

* Git의 모든 기능을 지원하는 것은 CLI 뿐이다.

<br>

# 1.5 Git 설치

## 리눅스에 설치

```
$ sudo yum install git
$ sudo apt-get install git
```

* [http://git-scm.com/download/linux](http://git-scm.com/download/linux)

## 맥에 설치

* Xcode Command Line Tools
* [http://git-scm.com/download/mac](http://git-scm.com/download/mac)
* [http://mac.github.com](http://mac.github.com)

## 윈도우에 설치

* [http://git-scm.com/download/win](http://git-scm.com/download/win)
* [https://git-for-windows.github.io](https://git-for-windows.github.io)
* [http://windows.github.com](http://windows.github.com)

<br>

# 1.6 Git 최초 설정

### git config 도구

각 설정은 역순으로 우선시 된다.

1. `/etc/gitconfig`: 시스템의 모든 사용자와 모든 저장소에 적용되는 설정, `git config --system` 옵션으로 이 파일을 읽고 쓸 수 있음
2. `~/.gitconfig`, `~/.config/git/config`: 특정 사용자에게만 적용되는 설정, `git config --global` 옵션으로 이 파일을 읽고 쓸 수 있음
3. `.git/config`: Git 디렉터리에 있고 특정 저장소(혹은 현재 작업중인 프로젝트)에만 적용

<br>

## 사용자 정보

사용자 이름과 이메일 주소를 설정

* 한번 커밋한 후에는 정보를 변경할 수 없다.
* 프로젝트마다 다른 이름과 이메일 주소를 사용하고 싶으면 `--global` 옵션을 빼고 실행한다.

```
$ git config --global user.name "John Doe"
$ git config --global user.email "johndoe@example.com"
```

<br>

## 편집기

```
$ git config --global core.editor emacs
```

<br>

## 설정 확인

```
$ git config --list
user.name=John Doe
user.email=johndoe@example.com
color.status=auto
color.branch=auto
color.interactive=auto
color.diff=auto
...
```

### 특정 Key에 대해 어떤 값을 사용하는지 확인

```
$ git config user.name
John Doe
```

<br>

# 1.7 도움말 보기

```
$ git help <verb>
$ git <verb> --help
$ man git-<verb>
```