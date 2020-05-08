# 1 시작하기

## 1.6 Git 최초 설정

### 1.6.1 사용자 정보

```shell
$ git config --global user.name "Sungmin.Seo"
$ git config --global user.email "esem.seo@gmail.com"
```

> 프로젝트마다 다른 이름과 이메일 주소를 사용하고 싶으면 `--global` 옵션을 빼고 실행


### 1.6.2 편집기

```shell
$ git config --global core.editor gvim
```


### 1.6.3 설정 확인

```shell
$ git config --list
$ git config <key>

$ git config user.name
```


## 1.7 도움말 보기

```shell
$ git help <verb>
$ git <verb> --help
$ man git-<verb>

$ git help config
```



# 2 Git의 기초

## 2.1 Git 저장소 만들기

### 2.1.1 기존 디렉터리를 Git 저장소로 만들기

```shell
$ git init
```


### 2.1.2 기존 저장소를 Clone하기

```shell
$ git clone https://github.com/esesem/ProGit
$ git clone https://github.com/esesem/ProGit ProGitNote
```


## 2.2 수정하고 저장소에 저장하기

### 2.2.1 파일의 상태 확인하기

```shell
$ git status
```


### 2.2.2 파일을 새로 추적하기

```shell
$ git add <file>
```


### 2.2.3 Modified 상태의 파일을 Stage하기

```shell
$ git add <file>
```


### 2.2.4 파일 상태를 짤막하게 확인하기

```shell
$ git status -s
```


### 2.2.5 파일 무시하기

#### `.gitignore` 파일

```shell
*.a             # 확장자가 .a인 파일 무시
!lib.a          # lib.a는 무시하지 않음
/TODO           # 현재 디렉터리의 TODO 파일 무시, subdir/TODO처럼 하위 디렉터리에 있는 파일은 무시하지 않음
build/          # build/ 디렉터리에 있는 모든 파일 무시
doc/*.txt       # doc/notes.txt 파일은 무시, doc/server/arch.txt 파일은 무시하지 않음
doc/**/*.pdf    # doc 디렉터리 아래의 모든 .pdf 파일 무시
```


### 2.2.6 Staged와 Unstaged 상태의 변경 내용을 보기

```shell
$ git diff
$ git diff --staged

```