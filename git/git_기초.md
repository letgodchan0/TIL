# git

- 분산 버전 관리 시스템(DVCS)
- 보통 폴더를 들어가서 클릭하고 삭제하는 등 폴더를 관리할 때 기본적으로 GUI 방식을 사용하지만, git은 CLI 방식으로도 사용이 가능하다.
  - Command Line Interface
  - git으로 관리하고 싶은 폴더에 들어가 마우스 우클릭 후 GIT Bash Here로 들어가면 된다.




## 1. git 저장소 만들기

```bash
$ git init
Initialized empty Git repository in C:/Users/livem/OneDrive/바탕 화면/first/.git/
(master) $
```

- `.git` 폴더가 생성 => 버전이 기록되는 저장소
  - 해당 폴더를 지우게 되면 모든 버전이 삭제되기 때문에 주의!
  - 이 폴더에서 파일들의 버전이 기록되고 저장된다.
- `$ git init` 을 하게 되면  `(master)`가 생기게 된다.
  - `livem@DESKTOP-V69ULCI MINGW64 ~/Desktop/test (master)`




## 2. git 파일의 라이프 사이클

[관련 문서 - Git 기초](https://git-scm.com/book/ko/v2/%EC%8B%9C%EC%9E%91%ED%95%98%EA%B8%B0-Git-%EA%B8%B0%EC%B4%88)

![img](git_%EA%B8%B0%EC%B4%88.assets/image.png)

내가 조작을 하고자 하는 working directory (working tree)에서 `add` 명령어를 통해 파일이나 폴더를 Staging Area (Index)로 보내고(?) `commit` 명령어를 통해 Staging Area에 있는 파일이나 폴더들을 .git 폴더로 보내고 이 폴더에서 버전별로 파일이나 폴더들을 관리하는 거라고 생각하면 된다.  이후 `push` 또는 `pull` 명령어를 통해 원격저장소 (git hub)와 연결된다.

### git에서 관리하는 파일 변경사항 

[관련 문서 - Git 수정하고 저장소에 저장하기](https://git-scm.com/book/ko/v2/Git%EC%9D%98-%EA%B8%B0%EC%B4%88-%EC%88%98%EC%A0%95%ED%95%98%EA%B3%A0-%EC%A0%80%EC%9E%A5%EC%86%8C%EC%97%90-%EC%A0%80%EC%9E%A5%ED%95%98%EA%B8%B0)

![img](git_%EA%B8%B0%EC%B4%88.assets/image-16440619556152.png)



- untracked: 커밋에 포함된 적 없는 파일
- tracked: 커밋에 포함된 적 있는 파일
  - modified: 커밋에 비해서 수정된 경우
  - staged: 커밋 되기 전 목록에 추가되어 있는 경우(현재 파일이 Staging area에 있다고 생각해보자)
  - commited: 커밋 된 상태

## 3. 버전 기록하기

### add

> Add file contents to the index

```bash
$ git add 파일명
$ git add a.txt
$ git add my_folder/
$ git add a.txt b.txt

add 취소하기
$ git restore --staged 파일명
```

- add만 한 파일은 staging area에 있지만 저장은 .git 폴더에서 하기 때문에 폴더에서 삭제한다면 되돌릴 수 없다

### commit

> Record changes to the repository

```bash
$ git commit -m "커밋메세지"

commit 메세지 변경, 주의해서 사용!
$ git commit --amend
```

- 커밋 메시지는 항상 버전의 내용(변경사항)에 대해서 나타낼 수 있도록 기록한다.

- a.txt 수정이 완료되어서 add를 하면 파일이 staging area로 이동하고 commit을 하면 repository (.git 폴더)로 이동하게 된다. 

- 각 commit은 고유한 값을 가지고 있고, 다른 경우 다른 commit이다.

## 4. git 원격저장소 활용(Git hub)

### 조회

```bash
$ git remote -v
origin  https://github.com/letgodchan0/first.git (fetch) 
origin  https://github.com/letgodchan0/first.git (push)
```

- fetch: 받아오는 것
- push: 보내는 것



### 추가

``` bash
$ git remote add <원격저장소이름> <url>
$ git remote add origin https://github.com/username/repository.git
```

- `origin` : 일반적으로 많이 활용되는 원격저장소 이름
- 연결하려고 하는 repository의 주소를 입력한다.



### 삭제

```bash
$ git remote rm <원격저장소이름>
$ git remote rm origin
```



### 원격 저장소 push

> Update remote refs along with associated objects

```bash
$ git push <원격저장소이름> <브랜치이름>
$ git push origin master
```

- `.git` 폴더에 있는 최신 버전의 파일들을 원격저장소에서 저장(?)하기 위한 명령어

### 원격 저장소 pull

> Fetch from and integrate with another repository or a local

```bash
$ git pull <원격저장소 이름> <브랜치 이름>
$ git pull origin master
```

- 원격저장소에서 혹시 commit을 하거나 변경사항이 있으면 그 사항들을 그대로 내 로컬에 적용하기 위한 명령어

### 원격 저장소 clone

> Clone a repository into a new directory

```bash
$ git clone <원격저장소 주소>
$ git clone https://github.com/username/repository.git
```

- 원격저장소에서 새로운 repository를 만들거나 다른 사람의 repository를 가져와서 관리하고 싶을 때 사용하는 명령어



## 5. git 기본 명령어

### Status

파일의 add 상태를 확인해 볼 수 있는 명령어

```bash
$ git status

# 일반적으로 아무것도 하지 않았을 때 상태 (모든 버전이 커밋이 완료된 상태)ㅎ
On branch main
nothing to commit, working tree clean

# 커밋 할 변경사항들 (Staging area에 있어)
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        deleted:    b.txt
        
# 커밋을 위해 준비되지 않은 변경사항 (Staging area에 없고 Working directory에 있어)
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   a.txt
        
# 트래킹되지 않은 파일들 (Working directory, 생에 처음으로 commit되길 기다리고 있음)
Untracked files:
  (use "git add <file>..." to include in what will be committed)
        c.txt
```

- 파일을 조작을 하는 방법은 총 4가지 있음
  - 생성 Create
  - ~~읽기 Read~~ , 거의 필요가 없음
  - 수정 Update
  - 삭세 Delete

### log

- commit의 버전들을 볼 수 있는 명령어

```bash
$ git log
commit b117d36373787c806f528d01ec76450f65a1aed4 (HEAD -> master)
Author: letgodchan0 <letgodchan0@gmail.com>
Date:   Thu Jan 13 12:29:28 2022 +0900

    Initial commit
    
로그에서 출력되는 버전 간의 차이점을 출력하고 싶을 때
$ git log -p

```



### restore

- staging area에 있는 파일을 내리는 것

~~~bash
$ git restore --staged 파일명
~~~



### touch

- 해당 폴더안에 파일을 만들 수 있는 명령어

```bash
# a.txt 파일이 생성된다
touch a.txt

```



# 확인해야할 사항

### commit 충돌

![Untitled](git_%EA%B8%B0%EC%B4%88.assets/Untitled.png)



- 원격 저장소에서 README.MD 파일을 수정하는 등 commit을 변경을 하고 로컬파일에서 파일을 변경하고 push를 하게 되면 commit이 충돌하게 된다.  따라서 `git pull origin master'` 명령어를 통해 원격저장소에서의 최신 버전을 로컬에 적용시켜주어야 할 필요가 있다.
- 원격저장소에서 commit을 하는 것은 최대한 지양을 하자!



# 요약

~~~bash
$ git init	# 해당 폴더를 깃으로 관리하겠다

git config --global user.email "여러분들의 이메일"
git config --global user.name "영어id"

$ git status # 해당 폴더의 git의 관리 상태를 보겠다.
$ touch README.md # 해당 폴더에 README 파일 만들기

$ git add README.MD		# README 파일을 git 관리에 추가하겠다.
$ git commit -m "원하는 커밋 메세지"	# add한 파일들의 버전을 로컬에서 관리하겠다.

$ git remote add origin 원격자정소 주소	# 원격저장소와 해당 폴더를 연결하겠다.

$ git push origin master	# master branch에 push 하겠다. (원격저장소에 저장하겠다.)
$ git pull origin master	# 원격저장소에서 커밋한 내용을 로컬에 반영하겠다

~~~

