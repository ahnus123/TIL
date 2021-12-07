# Git add / commit / push / reset 취소

## Git add 취소 (파일 상태를 Unstage로 변경)

+ `git reset HEAD [file]` : `git add`를 사용하여 일부 파일을 Staging Area에 추가한 경우, 해당 파일을 다시 Unstage로 변경 (파일명 생략 시, 전체 파일 취소됨)

  ```bash
  # 파일 상태 확인
  $ git status
  
  # Staging Area에 추가한 파일을 Unstage로 변경
  # git reset HEAD (파일명)
  $ git reset HEAD README.md
  ```

</br>

## Git commit 취소

### commit 취소

1. `git reset --soft HEAD^` : commit 취소 + 해당 파일들을 staged 상태로 working directory에 보존
2. `git reset --mixed HEAD^` : commit 취소 + 해당 파일들을 unstaged 상태로 working directory에 보존
3. `git reset --hard HEAD^` : commit 취소 + 해당 파일들을 staged 상태로 working directory에서 삭제

```bash
# 1. commit 취소 + 해당 파일들을 staged 상태로 working directory에 보존
$ git reset --soft HEAD^

# 2. commit 취소 + 해당 파일들을 unstaged 상태로 working directory에 보존
$ git reset --mixed HEAD^    # 기본 옵션
$ git reset HEAD^    # 위와 동일
$ git reset HEAD~2    # 마지막 2개의 commit 취소

# 3. commit 취소 + 해당 파일들을 staged 상태로 working directory에서 삭제
$ git reset --hard HEAD^
```

### commit message 변경

+ `git commit -ament` : git message 변경

  ```bash
  $ git commit -ament
  ```

  + reset 옵션
    + -soft : index 보존(add한 상태 & staged 상태) + working directory의 파일 보존 → 모두 보존
    + -mixed : index 취소(add하기 전 상태 & unstaged 상태) + working directory의 파일 보존 (default)
    + -hard : index 취소(add 하기 전 상태 & unstaged 상태) + working directory의 파일 삭제 → 모두 삭제

### 원격 저장소의 마지막 commit 상태로 되돌리기

+ `git reset --hard HEAD` : working directory를 원격 저장소의 마지막 commit 상태로 되돌리기

  + 단, 원격 저장소에 있는 마지막 commit 이후의 working directory와 add했던 파일들이 모두 사라짐

  ```bash
  # working directory를 원격 저장소의 마지막 commit 상태로 되돌림
  $ git reset --hard HEAD
  ```

</br>

## Git push 취소

+ 자신의 local 상태를 remote에 강제로 덮어쓰기
  - 되돌아간 commit 이후의 모든 commit이 사라짐

1. working directory에서 commit 되돌리기

   - 가장 최근의 commit을 취소 + working directory를 되돌리기

     ```bash
     # 가장 최근의 commit을 취소 (default : --mixed)
     $ git reset HEAD^
     
     # reflog(브랜치와 HEAD가 지난 몇 달 동안 가리켰었던 commit) 목록 확인
     $ git reflog
     or
     $ git log -g
     
     # 원하는 시점으로 working directory를 되돌림
     $ git reset HEAD@{number}
     or
     $ git reset [commit id]
     ```

2. 되돌려진 상태에서 다시 commit

   ```bash
   $ git commit -m "(commit 메세지)"
   ```

3. 원격 저장소에 강제로 push

   ```bash
   $ git push origin [branch명] -f
   or
   $ git push origin +[branch명]
   ```

</br>

## Git reset 취소

* `git reset` : reset하기 이전의 HEAD로 이동

  ```bash
  # reflog(브랜치와 HEAD가 지난 몇 달 동안 가리켰었던 commit) 목록 확인
  $ git reflog
  # Output
  e1e0a83 (HEAD -> master) HEAD@{0}: reset: moving to HEAD^
  bb6a86d (origin/master) HEAD@{1}: commit: [study] code refactoring
  e1e0a83 (HEAD -> master) HEAD@{2}: pull: Fast-forward
  ...
  
  # 원하는 시점으로 working directory를 되돌림
  $ git reset --hard HEAD@{1}    # ""[study] code refactoring" commit 상태로 되돌림
  ```

</br>

## untracked 파일 삭제

+ `git clean` : 추적중이지 않은 파일만 삭제. 즉, .gitignore에 명시하여 무시되는 파일을 삭제하지 않음

  ```bash
  $ git clean -f     # 디렉터리를 제외한 파일들만 삭제
  $ git clean -f -d    # 디렉터리까지 삭제
  $ git clean -f -d -x    # 무시된 파일까지 삭제
  ```

  + 옵션
    + -d : 디렉터리까지 삭제
    + -x : 무시된 파일(.DS_Store or .git ignore에 등록된 퐉장자 파일들)까지 모두 삭제
    + -n : 가상으로 실행해보고 어떤 파일들이 지워지는지 알려줌



</br></br>

[참고] https://gmlwjd9405.github.io/2018/05/25/git-add-cancle.html