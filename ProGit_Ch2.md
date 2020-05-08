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



### 2.2.7 변경사항 커밋하기

수정한 것을 커밋하기 위해 Staging Area에 파일을 정리했다.  
Unstage 상태의 파일은 커밋되지 않는다. Git은 생성하거나 수정하고 나서 `git add` 명령으로 추가하지 않은 파일은 커밋하지 않는다. 그 파일은 여전히 Modified 상태로 남아있다. 커밋하기 전에 `git status` 명령으로 모든 것이 Staged 상태인지 확인할 수 있다.

```shell
$ git commit                                            # 자동으로 커밋 메시지 생성
$ git commit -v                                         # 편집기에 diff 메시지 추가
$ git commit -m "Story 182: Fix benchmarks for speed"   # 인라인으로 메시지 첨부
[master 463dc4f] Story 182: Fix benchmarks for speed
 2 files changed, 2 insertions(+)
 create mode 100644 README
```

* `(master)` 브랜치에 커밋했고 체크섬은 `(463dc4f)`라고 알려준다.
* 수정한 파일이 몇 개이고 삭제됐거나 추가된 라인이 몇 라인인지 알려준다.

> Git은 Staging Area에 속한 스냅샷을 커밋한다. 수정은 했지만 아직 Staging Area에 넣지 않은 것은 다음에 커밋할 수 있다.  
> 커밋할 때마다 프로젝트의 스냅샷을 기록하기 때문에 나중에 스냅샷끼리 비교하거나 예전 스냅샷으로 되돌릴 수 있다.



### 2.2.8 Staging Area 생략하기

`git commit` 명령을 실행할 때 `-a` 옵션을 추가하면 Tracked 상태의 파일을 자동으로 Staging Area에 넣는다. 그래서 `git add` 명령을 실행하는 수고를 덜 수 있다.

```shell
$ git commit -a -m 'added new benchmarks'
[master 83e38c7] added new benchmarks
 1 file changed, 5 insertions(+), 0 deletions(-)
```



### 2.2.9 파일 삭제하기

Git에서 파일을 제거하려면 `git rm` 명령으로 Tracked 상태의 파일을 삭제한 후에(정확하게는 Staging Area에서 삭제하는 것) 커밋해야 한다. 이 명령은 워킹 디렉터리에 있는 파일도 삭제하기 때문에 실제로 파일도 지워진다.

Git 명령을 사용하지 않고 단순히 워킹 디렉터리에서 파일을  삭제하고 `git status` 명령으로 상태를 확인하면 Git은 현재 "Changes not staged for commit"(즉, Unstaged 상태)라고 표시해준다.

```shell
$ git rm grit.gemspec
rm 'grit.gemspec'
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

     deleted:    grit.gemspec
```

`git rm` 명령을 실행하면 삭제한 파일은 Staged 상태가 된다.  
커밋하면 파일은 삭제되고 Git은 이 파일을 더는 추적하지 않는다. 이미 파일을 수정했거나, 수정한 파일을 Index(Staging Area)에 추가했다면 `-f` 옵션을 주어 강제로 삭제해야 한다. 이 점은 실수로 데이터를 삭제하지 못하도록 하는 안전장치다. 커밋하지 않고 수정한 데이터는 Git으로 복구 할 수 없기 때문이다.

Staging Area에서만 제거하고 워킹 디렉터리에 있는 파일은 지우지 않고 남겨둘 수 있다. 이것은 `.gitignore` 파일에 추가하는 것을 빼먹었거나, 대용량 로그 파일이나 컴파일된 파일인 `.a` 파일 같은 것을 실수로 추가했을 때 아주 유용하다. `--cached` 옵션을 사용하여 명령을 실행한다.

```shell
$ git rm --cached README
```



### 2.2.10 파일 이름 변경하기

Git은 다른 VCS와는 달리 파일 이름의 변경이나 파일의 이동을 명시적으로 관리하지 않는다.

```shell
$ git mv README.md README
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

    renamed:    README.md -> README
```

`git mv` 명령은 아래 명령어를 수행한 것과 완전 똑같다.

```shell
$ mv README.md README
$ git rm README.md
$ git add README
```

> `git mv`는 일종의 단축 명령어이다. Git의 `mv` 명령은 편리하게 명령을 세 번 실행해주는 것 뿐이다.




## 2.3 커밋 히스토리 조회하기

Git에는 히스토리를 조회하는 명령어인 `git log`가 있다.

```shell
$ git log
commit ca82a6dff817ec66f44342007202690a93763949
Author: Scott Chacon <schacon@gee-mail.com>
Date:   Mon Mar 17 21:52:11 2008 -0700

    changed the version number

commit 085bb3bcb608e1e8451d4b2432f8ecbe6306e7e7
Author: Scott Chacon <schacon@gee-mail.com>
Date:   Sat Mar 15 16:40:33 2008 -0700
```

특별한 아규먼트 없이 `git log` 명령을 실행하면 저장소의 커밋 히스토리를 시간순으로 보여준다. 즉, 가장 최근의 커밋이 가장 먼저 나온다. 이어서 각 커밋의 SHA-1 체크섬, 저자 이름, 저자 이메일, 커밋한 날짜, 커밋 메시지를 보여준다.


#### `-p`

각 커밋의 diff 결과를 보여준다.


#### `-2`

최근 두 개의 결과만 보여주는 옵션이다.  
이 옵션은 직접 diff를 실핸한 것과 같은 결과를 출력하기 때문에 동료가 무엇을 커밋했는지 리뷰하고 빨리 조회하는 데 유용하다.


#### `--stat`

각 커밋의 통계 정보를 조회할 수 있다.  
어떤 파일이 수정됐는지, 얼마나 많은 파일이 변경됐는지, 또 얼마나 많은 라인을 추가하거나 삭제했는지 보여준다. 요약정보는 가장 뒤쪽에 보여준다.


#### `--pretty`

히스토리 내용을 보여줄 때 기본 형식 이외에 여러 가지 중 하나를 선택할 수 있다.

* `oneline`: 각 커밋을 한 라인으로 보여준다. 많은 커밋을 한 번에 조회할 때 유용하다.
* `short`, `full`, `fuller`: 정보를 조금씩 가감해서 보여준다.
* `format`: 나만의 포맷으로 결과를 출력하고 싶을 때 사용한다. 결과를 다른 프로그램으로 파싱하고자 할 때 유용하다.

##### `git log --pretty=format`에 쓸 수 있는 몇 가지 유용한 옵션

| 옵션  | 설명                              |
| ----- | --------------------------------- |
| `%H`  | 커밋 해시                         |
| `%h`  | 짧은 길이 커밋 해시               |
| `%T`  | 트리 해시                         |
| `%t`  | 짧은 길이 트리 해시               |
| `%P`  | 부모 해시                         |
| `%p`  | 짧은 길이 부모 해시               |
| `%an` | 저자 이름                         |
| `%ae` | 저자 메일                         |
| `%ad` | 저자 시각(형식은 -date=옵션 참고) |
| `%ar` | 저자 상대적 시각                  |
| `%cn` | 커미터 이름                       |
| `%ce` | 커미터 메일                       |
| `%cd` | 커미터 시각                       |
| `%cr` | 커미터 상대적 시가                |
| `%s`  | 요약                              |


#### `--graph`

`oneline`과 `format` 옵션은 `--graph` 옵션과 함께 사용할 때 더 빛난다. 이 명령은 브랜치와 머지 히스토리를 보여주는 아스키 그래프를 출력한다.

##### `git log` 주요 옵션

| 옵션              | 설명                                                         |
| ----------------- | ------------------------------------------------------------ |
| `-p`              | 각 커밋에 적용된 패치를 보여준다.                            |
| `--stat`          | 각 커미셍서 수정된 파일의 통계정보를 보여준다.               |
| `--shortstat`     | `--stat` 명령의 결과 중에서 수정한 파일, 추가된 라인, 삭제된 라인만 보여준다. |
| `--name-only`     | 커밋 정보 중에서 수정된 파일의 목록만 보여준다.              |
| `--name-status`   | 수정된 파일의 목록을 보여줄 뿐만 아니라 파일을 추가한 것인지, 수정한 것인지, 삭제한 것인지도 보여준다. |
| `--abbrev-commit` | 40자짜리 SHA-1 체크섬을 전부 보여주는 것이 아니라 처음 몇 자만 보여준다. |
| `--relative-date` | 정확한 시간을 보여주는 것이 아니라 "2주 전"처럼 상대적인 형식으로 보여준다. |
| `--graph`         | 브랜치와 Merge 히스토리 정보까지 아스키 그래프로 보여준다.   |
| `--pretty`        | 지정한 형식으로 보여준다. 이 옵션에는 `oneline`, `short`, `full`, `fuller`, `format`이 있다. `format`은 원하는 형식으로 출력하고자 할 때 사용한다. |



### 2.3.1 조회 제한조건

##### `git log` 조회 범위를 제한하는 옵션

| 옵션                  | 설명                                              |
| --------------------- | ------------------------------------------------- |
| `-(n)`                | 최근 n개의 커밋만 조회한다.                       |
| `--since`, `--after`  | 명시한 날짜 이후의 커밋만 검색한다.               |
| `--until`, `--before` | 명시한 날짜 이전의 커밋만 조회한다.               |
| `--author`            | 입력한 저자의 커밋만 보여준다.                    |
| `--committer`         | 입력한 커미터의 커밋만 보여준다.                  |
| `--grep`              | 커밋 메시지 안의 텍스트를 검색한다.               |
| `-S`                  | 커밋 변경(추가/삭제) 내용 안의 텍스트를 검색한다. |

* `author`와 `grep` 옵션을 함께 사용하여 모두 만족하는 커밋을 찾으려면 `--all-match` 옵션도 반드시 함께 사용해야 한다.
* 파일 경로로 검색하는 옵션이 있는데 디렉터리나 파일 이름을 사용하여 그 파일이 변경된 log의 결과를 검색할 수 있다. 이 옵션은 `--`와 함께 경로 이름을 사용하는데 명령어 끝 부분에 쓴다.




## 2.4 되돌리기

한 번 되돌리면 복구할 수 없기에 주의해야 한다. Git을 사용하면 실수 중 복구하지 못할 게 거의 없지만, 되돌린 것은 복구할 수 없다.

#### 완료한 커밋을 수정해야 할 때

너무 일찍 커밋했거나 어떤 파일을 빼먹었을 때 그리고 커밋 메시지를 잘못 적었을 때 그렇다. 다시 커밋하고 싶으면 `--amend` 옵션을 사용한다.

```shell
$ git commit --amend
```

* Staging Area를 사용하여 커밋한다.
* 마지막으로 커밋하고나서 수정한 것이 없다면 조금 전에 한 커밋과 모든 것이 같고, 커밋 메시지만 수정한다. 메시지를 수정하지 않고 그대로 커밋해도 기존의 커밋을 덮어쓴다.

커밋을 했는데 Stage하는 것을 깜빡하고 빠뜨린 파일이 있으면 아래와 같이 고칠 수 있다.

```shell
$ git commit -m 'initial commit'
$ git add forgotten_file
$ git commit --amend
```

* 두 번째 커밋은 첫 번째 커밋을 덮어쓴다.



### 2.4.1 파일 상태를 Unstage로 변경하기

`git reset HEAD <file>...` 명령으로 Unstage 상태로 변경할 수 있다.

> `git reset` 명령을 `--hard` 옵션과 함께 사용하면 워킹 디렉터리 파일까지 수정되기에 조심해야 한다. `--hard` 옵션만 사용하지 않는다면 `git reset` 명령은 Staging Area의 파일만 조작하기 때문에 위험하지 않다.



### 2.4.2 Modified 파일 되돌리기

수정하고 나서 다시 최근 커밋된 버전으로 되돌리는 방법이 무얼까? `git status` 명령이 친절하게 알려준다.

```shell
git checkout -- <file>...
```

> `git checkout -- [file]` 명령은 꽤 위험한 명령이라는 것을 알아야 한다. 원래 파일로 덮어썼기 때문에 수정한 내용은 전부 사라진다. 수정한 내용이 진짜 마음에 들지 않을 때만 사용하자.  
변경한 내용을 쉽게 버릴 수는 없고 하지만 당장은 되돌려야만 하는 상황이라면 `Stash`와 Branch를 사용하자.




## 2.5 리모트 저장소

리모트 저장소는 인터넷이나 네트워크 어딘가에 있는 저장소를 말한다. 다른 사람들과 함께 일한다는 것은 리모트 저장소를 관리하면서 데이터를 거기에 Push하고 Pull하는 것이다. 리모트 저장소를 관리한다는 것은 저장소를 추가, 삭제하는 것뿐만 아니라 브랜치를 관리하고 추적할지 말지 등을 관리하는 것을 말한다.



### 2.5.1 리모트 저장소 확인하기

`git remote` 명령으로 현재 프로젝트에 등록된 리모트 저장소를 확인할 수 있다.

* 리모트 저장소의 단축 이름을 보여준다.
* `-v` 옵션을 주어 단축 이름과 URL을 함께 볼 수 있다.
* 리모트 저장소가 여러 개 있다면 등록된 전부를 보여준다.
* 리모트 저장소가 여러 개 등록되어 있으면 다른 사람이 기여한 내용(Contributions)을 쉽게 가져올 수 있다.



### 2.5.2 리모트 저장소 추가하기

`git remote add [단축 이름] [url]` 명령으로 실행한다.

```shell
$ git remote
origin
$ git remote add pb https://github.com/paulboone/ticgit
$ git remote -v
origin  https://github.com/schacon/ticgit (fetch)
origin  https://github.com/schacon/ticgit (push)
pb      https://github.com/paulboone/ticgit (fetch)
pb      https://github.com/paulboone/ticgit (push)
```

* URL 대신에 `pb`라는 이름을 사용할 수 있다.



### 2.5.3 리모트 저장소를 Pull하거나 Fetch하기

#### `git fetch`

```shell
$ git fetch [remote-name]
```

* 로컬에는 없지만, 리모트 저장소에는 있는 데이터를 모두 가져온다.
* 리모트 저장소의 모든 브랜치를 로컬에서 접근할 수 있어서 언제든지 Merge를 하거나 내용을 살펴볼 수 있다.
* 저장소를 Clone하면 명령은 자동으로 리모트 저장소를 "origin"이라는 이름으로 추가한다. 그래서 나중에 `git fetch origin`을 실행하면 Clone한 이후에 수정된 것을 모두 가져온다.
* `git fetch` 명령은 리모트 저장소의 데이터를 모두 로컬로 가져오지만, 자동으로 Merge하지 않는다. 그래서 로컬에서 하던 작업을 정리하고 나서 수동으로 Merge해야 한다.

#### `git pull`

* `git clone` 명령은 자동으로 로컬의 master 브랜치가 리모트 저장소의 master 브랜치를 추적하도록 한다.
* `git pull` 명령은 Clone한 서버에서 데이터를 가져오고 그 데이터를 자동으로 현재 작업하는 코드와 Merge시킨다.



### 2.5.4 리모트 저장소에 Push하기

프로젝트를 공유하고 싶을 때 Upstream 저장소에 Push할 수 있다. 이 명령은 `git push [리모트 저장소 이름] [브랜치 이름]`으로 단순하다.

```shell
$ git push origin master
```

* master 브랜치를 `origin` 서버에 Push한다.
* Clone한 리모트 저장소에 쓰기 권한이 있고, Clone하고 난 이후 아무도 Upstream에 Push하지 않았을 때만 사용할 수 있다. 먼저 다른 사람이 작업한 것을 가져와서 Merge한 후에 Push할 수 있다.



### 2.5.5 리모트 저장소 살펴보기