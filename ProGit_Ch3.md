# 3 Git 브랜치




## 3.1 브랜치란 무엇인가

Git은 데이터를 변경사항(Diff)으로 기록하지 않고 일련의 스냅샷으로 기록한다.  
커밋하면 Git은 현 Staging Area에 있는 데이터의 스냅샷에 대한 포인터, 저자나 커밋 메시지 같은 메타데이터, 이전 커밋에 대한 포인터 등을 포함하는 커밋 개체(커밋 Object)를 저장한다. 이전 커밋 포인터가 있어서 현재 커밋이 무엇을 기준으로 바뀌었는지를 알 수 있다. 최초 커밋을 제외한 나머지 커밋은 이전 커밋 포인터가 적어도 하나씩 있고 브랜치를 합친 Merge 커밋 같은 경우에는 이전 커밋 포인터가 여러 개 있다.

* 파일을 Stage하면 Git 저장소에 파일을 저장하고 (Git은 이것을 Blob이라고 부른다) Staging Area에 해당 파일의 체크섬을 저장한다.
* `git commit`으로 커밋하면 먼저 루트 디렉터리와 각 하위 디렉터리의 트리 개체를 체크섬과 함께 저장소에 저장한다.
* 그 다음에 커밋 개체를 만들고 메타데이터와 루트 디렉터리 트리 개체를 가리키는 포인터 정보를 커밋 개체에 넣어 저장한다. 그래서 필요하면 언제든지 스냅샷을 다시 만들 수 있다.
* 이 작업을 마치고 나면 Git 저장소에는 다섯 개의 데이터 개체가 생긴다.
  * 각 파일에 대한 Blob 세 개
  * 파일과 디렉터리 구조가 들어 있는 트리 개체 하나
  * 메타데이터와 루트 트리를 가리키는 포인터가 담긴 커밋 개체 하나

Git의 브랜치는 커밋 사이를 가볍게 이동할 수 있는 어떤 포인터 같은 것이다. 기본적으로 Git은 master 브랜치를 만든다. 처음 커밋하면 이 master 브랜치가 생성된 커밋을 가리킨다. 이후 커밋을 만들면 브랜치는 자동으로 가장 마지막 커밋을 가리킨다.



### 3.1.1 새 브랜치 생성하기

```shell
git branch testing
```

* 새로 만든 브랜치도 지금 작업하고 있던 마지막 커밋을 가리킨다.

    ![그림 3-4 한 커밋 히스토리를 가리키는 두 브랜치](https://raw.githubusercontent.com/esesem/ProGit/master/Figure/3-4.png)

Git은 'HEAD'라는 특수한 포인터가 있다. 이 포인터는 지금 작업하는 로컬 브랜치를 가리킨다. 브랜치를 새로 만들었지만, Git은 아직 `master` 브랜치를 가리키고 있다. `git branch` 명령은 브랜치를 만들기만 하고 브랜치를 옮기지 않는다.

![그림 3-5 현재 작업 중인 브랜치를 가리키는 HEAD](https://raw.githubusercontent.com/esesem/ProGit/master/Figure/3-5.png)

`git log` 명령에 `--decorate` 옵션을 사용하면 쉽게 브랜치가 어떤 커밋을 가리키는지도 확인할 수 있다.

```shell
$ git log --oneline --decorate
f30ab (HEAD, master, testing) add feature #32 - ability to add new
34ac2 fixed bug #1328 - stack overflow under certain conditions
98ca9 initial commit of my project
```


### 3.1.2 브랜치 이동하기

`git checkout` 명령으로 다른 브랜치로 이동할 수 있다.

```shell
$ git checkout testing
```

* HEAD는 testing 브랜치를 가리킨다.

    ![그림 3-6 HEAD는 testing 브랜치를 가리킴](https://raw.githubusercontent.com/esesem/ProGit/master/Figure/3-6.png)

```shell
$ vim test.rb
$ git commit -a -m "made a change"
```

* `testing` 브랜치는 앞으로 이동했다.
* 하지만 `master` 브랜치는 여전히 이전 커밋을 가리킨다.

    ![그림 3-7 HEAD가 가리키는 testing 브랜치가 새 커밋을 가리킴](https://raw.githubusercontent.com/esesem/ProGit/master/Figure/3-7.png)

```shell
$ git checkout master
```

* `master` 브랜치가 가리키는 커밋을 HEAD가 가리키게 하고 워킹 디렉터리의 파일도 그 시점으로 되돌려 놓았다.
* 앞으로 커밋을 하면 다른 브랜치의 작업들과 별개로 진행되기 때문에 `testing` 브랜치에서 임시로 작업하고 원래 `master` 브랜치로 돌아와서 하던 일을 계속할 수 있다.

    ![그림 3-8 HEAD가 checkout한 브랜치로 이동함](https://raw.githubusercontent.com/esesem/ProGit/master/Figure/3-8.png)
