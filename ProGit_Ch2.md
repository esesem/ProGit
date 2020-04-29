# 2 Git의 기초




## 2.1 Git 저장소 만들기



### 2.1.1 기존 디렉터리를 Git 저장소로 만들기

```shell
$ git init
```

* `.git`이라는 하위 디렉터리를 만듦
* `.git` 디렉터리에는 저장소에 필요한 뼈대 파일(skeleton)이 들어 있음


#### 파일 버전 관리 시작: 파일을 추가하고 커밋

```shell
$ git add *.c
$ git add LICENSE
$ git commit -m 'initial project version'
```



### 2.2.2 기존 저장소를 Clone하기


#### 다른 프로젝트에 참여하려거나(contribute) Git 저장소를 복사하고 싶을 때

* `git clone` 사용
  * 서버에 있는 거의 모든 데이터를 복사
  * 프로젝트 히스토리를 전부 받아옴
  * 저장소의 데이터를 모두 가져와서 자동으로 가장 최신 버전을 Checkout

```shell
$ git clone https://github.com/libgit2/libgit2
```

* `libgit2` 디렉터리를 만들고 그 안에 `.git` 디렉터리를 만듦
* 저장소의 데이터를 모두 가져와서 자동으로 가장 최신 버전을 Checkout

```shell
$ git clone https://github.com/libgit2/libgit2 mylibgit
```

* `libgit2`이 아니라 다른 디렉터리 이름(`mylibgit`)으로 Clone


#### 다양한 프로토콜 지원

* `https://`
* `git://`
* SSH 프로토콜: `user@server:path/to/repo.git`




## 2.2 수정하고 저장소에 저장하기

> #### 워킹 디렉터리의 모든 파일은 크게 Tracked(관리대상임)와 Untracked(관리대상이 아님)로 나눔
> * Tracked 파일: 이미 스냅샷에 포함돼 있던 파일, 처음 저장소를 Clone하면 모든 파일은 Tacked 이면서 Unmodified 상태(아무것도 수정하지 않았기 때문)
>   * Unmodified: 수정하지 않음
>   * Modified: 수정함
>   * Staged: 커밋으로 저장소에 기록할 상태
> * Untracked 파일: 나머지 파일 모두, 워킹 디렉터리에 있는 파일 중 스냅샷에도 Staging Area에도 포함되지 않은 파일

![그림 2-1 파일의 라이프사이클](https://raw.githubusercontent.com/esesem/ProGit/master/Figure/2-1.png)

> 마지막 커밋 이후 아직 아무것도 수정하지 않은 상태에서 어떤 파일을 수정하면 Git은 그 파일을 Modified 상태로 인식한다. 커밋을 하기 위해서는 이 수정한 파일을 Staged 상태로 만들고, Staged 상태의 파일을 커밋한다. 이런 라이프사이클을 계속 반복한다.



### 파일의 상태 확인하기

> #### `git status` 명령 사용
> ```shell
> $ git status
> On branch master
> nothing to commit, working directory clean
> ```
> * 파일을 하나도 수정하지 않았다(Tracked나 Modified 상태인 파일이 없다는 의미).
> * Untracked 파일은 아직 없어서 목록에 나타나지 않는다.
> * 현재 작업 중인 브랜치를 알려주며 서버의 같은 브랜치로부터 진행된 작업이 없는 것을 나타낸다.
> * 기본 브랜치가 master이기 때문에 현재 브랜치 이름이 "master"로 나온다.
>
> #### 새로 만든 파일
> ```shell
> $ echo 'My Project' > README
> $ git status
> On branch master
>
> No commits yet
>
> Untracked files:
>   (use "git add <file>..." to include in what will be committed)
>        README
>
> nothing added to commit but untracked files present (use "git add" to track)
> ```
> * `README` 파일이 Untracked 상태라는 것을 말한다.

> Git은 Untracked 파일을 아직 스냅샷(커밋)에 넣어지지 않은 파일이라고 본다. 파일이 Tracked 상태가 되기 전까지는 Git은 절대 그 파일을 커밋하지 않는다. 일하면서 생성하는 바이너리 파일 같은 것을 커밋하는 실수는 하지 않게 된다.



### 파일을 새로 추적하기

<`git add` 명령 사용>

```shell
$ git add README
$ git status
On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
        new file:   README
```

* `README` 파일이 Tracked 상태이면서 커밋에 추가될 Staged 상태라는 것을 확인할 수 있다.

> "Changes to be commited"에 들어 있는 파일은 Staged 상태라는 것을 의미한다. 커밋하면 `git add`를 실행한 시점의 파일이 커밋되어 저장소 히스토리에 남는다.

> `git add` 명령은 파일 또는 디렉터리의 경로를 아규먼트로 받는다. 디렉터리면 아래에 있는 모든 파일들까지 재귀적으로 추가한다.



### Modified 상태의 파일을 Stage하기

"CONTRIBUTING.md"라는 파일을 수정하고 나서 `git status` 명령을 실행하면 결과는 아래와 같다.

```shell
On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
        new file:   README

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        CONTRIBUTING.md
```