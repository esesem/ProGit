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


#### 워킹 디렉터리의 모든 파일은 크게 Tracked(관리대상임)와 Untracked(관리대상이 아님)로 나눔

* Tracked 파일: 이미 스냅샷에 포함돼 있던 파일, 처음 저장소를 Clone하면 모든 파일은 Tacked 이면서 Unmodified 상태(아무것도 수정하지 않았기 때문)
  * Unmodified: 수정하지 않음
  * Modified: 수정함
  * Staged: 커밋으로 저장소에 기록할 상태
* Untracked 파일: 나머지 파일 모두, 워킹 디렉터리에 있는 파일 중 스냅샷에도 Staging Area에도 포함되지 않은 파일


#### 파일의 라이프사이클

![그림 2-1 파일의 라이프사이클](https://raw.githubusercontent.com/esesem/ProGit/master/Figure/2-1.png)

> 마지막 커밋 이후 아직 아무것도 수정하지 않은 상태에서 어떤 파일을 수정하면 Git은 그 파일을 Modified 상태로 인식한다. 커밋을 하기 위해서는 이 수정한 파일을 Staged 상태로 만들고, Staged 상태의 파일을 커밋한다. 이런 라이프사이클을 계속 반복한다.



### 2.2.1 파일의 상태 확인하기

#### `git status` 명령 사용

```shell
$ git status
On branch master
nothing to commit, working directory clean
```

* 파일을 하나도 수정하지 않았다(Tracked나 Modified 상태인 파일이 없다는 의미).
* Untracked 파일은 아직 없어서 목록에 나타나지 않는다.
* 현재 작업 중인 브랜치를 알려주며 서버의 같은 브랜치로부터 진행된 작업이 없는 것을 나타낸다.
* 기본 브랜치가 master이기 때문에 현재 브랜치 이름이 "master"로 나온다.


#### 새로 만든 파일

```shell
$ echo 'My Project' > README
$ git status
On branch master
Untracked files:
  (use "git add <file>..." to include in what will be committed)

    README

nothing added to commit but untracked files present (use "git add" to track)
```

* `README` 파일이 Untracked 상태라는 것을 말한다.

> Git은 Untracked 파일을 아직 스냅샷(커밋)에 넣어지지 않은 파일이라고 본다. 파일이 Tracked 상태가 되기 전까지는 Git은 절대 그 파일을 커밋하지 않는다. 일하면서 생성하는 바이너리 파일 같은 것을 커밋하는 실수는 하지 않게 된다.



### 2.2.2 파일을 새로 추적하기


#### `git add` 명령 사용

```shell
$ git add README
$ git status
On branch master
Changes to be committed:
  (use "git rm --cached <file>..." to unstage)

    new file:   README
```

* `README` 파일을 추적한다.
* `README` 파일이 Tracked 상태이면서 커밋에 추가될 Staged 상태라는 것을 확인할 수 있다.

> "Changes to be commited"에 들어 있는 파일은 Staged 상태라는 것을 의미한다. 커밋하면 `git add`를 실행한 시점의 파일이 커밋되어 저장소 히스토리에 남는다.  
> `git add` 명령은 파일 또는 디렉터리의 경로를 아규먼트로 받는다. 디렉터리면 아래에 있는 모든 파일들까지 재귀적으로 추가한다.



### 2.2.3 Modified 상태의 파일을 Stage하기


#### 파일을 수정하고 나서 `git status` 명령을 실행

```shell
$ git status
On branch master
Changes to be committed:
  (use "git rm --cached <file>..." to unstage)

    new file:   README

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

    modified:   CONTRIBUTING.md
```

* `CONTRIBUTING.md` 파일은 Tracked 상태이지만 아직 Staged 상태는 아니다.
* Staged 상태로 만들려면 `git add` 명령을 실행해야 한다.

> `git add` 명령은 파일을 새로 추적할 때도 사용하고 수정한 파일을 Staged 상태로 만들 때도 사용한다. Merge할 때 충돌 난 상태의 파일을 Resolve 상태로 만들 때도 사용한다.  
> add의 의미는 프로젝트에 파일을 추가한다기보다는 다음 커밋에 추가한다고 받아들이는 게 좋다.


#### 파일을 Staged 상태로 만들고 `git status` 명령으로 결과를 확인

```shell
$ git add CONTRIBUTING.md
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

    new file:   README
    modified:   CONTRIBUTING.md
```

* 두 파일 모두 Staged 상태이므로 다음 커밋에 포함된다.


#### 아직 더 수정해야 한다는 것을 알게 되어 바로 커밋하지 못하는 상황

```shell
$ vim CONTRIBUTING
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

    new file:   README
    modified:   CONTRIBUTING.md

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

    modified:   CONTRIBUTING.md
```

* `CONTRIBUTING.md`가 Staged 상태이면서 동시에 Unstaged 상태로 나온다.

> `git add` 명령을 실행하면 Git은 파일을 바로 Staged 상태로 만든다.  
> 이 시점에서 커밋을 하면 `git commit` 명령을 실행하는 시점의 버전이 커밋되는 것이 아니라 마지막으로 `git add` 명령을 실행했을 때의 버전이 커밋된다.  
> `git add` 명령을 실행한 후에 또 파일을 수정하면 `git add` 명령을 다시 실행해서 최신 버전을 Staged 상태로 만들어야 한다.



### 2.2.4 파일 상태를 짤막하게 확인하기

`git status -s` 또는 `git status --short`처럼 옵션을 주면 현재 변경한 상태를 짤막하게 보여준다.

```shell
$ git status -s
 M README
MM Rakefile
A  lib/git.rb
M  lib/simplegit.rb
?? LICENSE.txt
```

* `??`: 아직 추적하지 않는 새 파일
* `A`: Staged 상태로 추가한 파일 중 새로 생성한 파일
* `M`: 수정한 파일
* `MM`: Staged 상태로 추가한 후 또 내용을 변경해서 Staged이면서 Unstaged 상태인 파일



### 2.2.5 파일 무시하기

Git이 관리할 필요가 없는 파일(로그 파일이나 빌드 시스템이 자동으로 생성한 파일)을 무시하려면 `.gitignore` 파일을 만들고 그 안에 무시할 파일 패턴을 적는다.

* 아무것도 없는 라인이나, `#`로 시작하는 라인은 무시한다.
* 표준 Glob 패턴을 사용한다.
* 슬래시(`/`)로 시작하면 하위 디렉터리에 적용되지(recursivity) 않는다.
* 디렉터리는 슬래시(`/`)를 끝에 사용하는 것으로 표현한다.
* 느낌표(`!`)로 시작하는 패턴의 파일은 무시하지 않는다.

```shell
# 확장자가 .a인 파일 무시
*.a

# 윗 라인에서 확장자가 .a인 파일은 무시하게 했지만 lib.a는 무시하지 않음
!lib.a

# 현재 디렉터리에 있는 TODO 파일은 무시하고 subdir/TODO처럼 하위 디렉터리에 있는 파일은 무시하지 않음
/TODO

# build/ 디렉터리에 있는 모든 파일은 무시
build/

# doc/notes.txt 파일은 무시하고 doc/server/arch.txt 파일은 무시하지 않음
doc/*.txt

# doc 디렉터리 아래의 모든 .pdf 파일을 무시
doc/**/*.pdf
```

* [`.gitignore` 예제](https://github.com/github/gitignore)



### 2.2.6 Staged와 Unstaged 상태의 변경 내용을 보기

어떤 내용이 변경됐는지 살펴보려면 `git diff` 명령을 사용해야 한다.

* 수정했지만 아직 staged 상태가 아닌 파일을 비교해 볼 수 있다.
* 워킹 디렉터리에 있는 것과 Staging Area에 있는 것을 비교한다. 그래서 수정하고 아직 Stage하지 않은 것을 보여준다.
* 커밋하려고 Staging Area에 넣은 파일의 변경 부분을 보고 싶으면 `git diff --staged` 옵션을 사용한다. 이 명령은 저장소에 커밋한 것과 Staging Area에 있는 것을 비교한다.
* `git diff` 명령은 마지막으로 커밋한 후에 수정한 것들 전부를 보여주지 않는다. Unstaged 상태인 것들만 보여준다. 수정한 파일을 모두 Staging Area에 넣었다면 `git diff` 명령은 아무것도 출력하지 않는다.
* Stage한 후에 다시 수정해도 `git diff` 명령을 사용할 수 있다. 이때는 Staged 상태인 것과 Unstaged 상태인 것을 비교한다.
* Unstaged 상태인 변경 부분을 확인할 수 있다.
* Staged 상태인 파일은 `git diff --cached` 옵션으로 확인한다. `--staged`와 `--cached`는 같은 옵션이다.
* `git diff` 대신 `git difftool` 명령을 사용해서 emerge, vimdiff 같은 도구로 비교할 수 있다. `git difftool --tool-help` 명령은 사용 가능한 도구를 보여준다.