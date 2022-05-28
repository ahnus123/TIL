## Git Branch

### Local Branch

+ 브랜치 확인

  ```bash
  $ git branch
  ```

+ 브랜치 생성

  ```bash
  $ git branch [branch명]
  ```

+ 브랜치 이동

  ```bash
  $ git checkout [branch명]
  
  # git ver2.23 이상
  $ git switch [branch명]   # Switch branches
  $ git restore [branch명]  # Restore working tree files
  ```

+ 브랜치 생성 및 이동

  ```bash
  $ git checkout -b [branch명]
  
  # git ver2.23 이상
  $ git switch -c [branch명]   # Switch branches
  $ git switch -t origin/[branch명]  # 원격브랜치와 동일한 이름으로 브랜치 생성 및 변경
  ```

+ 브랜치 삭제

  ```bash
  $ git branch -d [branch명]
  $ git branch -D [branch명]  # -D : 강제삭제 옵션
  ```

+ 브랜치 이름 변경

  ```bash
  $ git branch -m [원래 branch명] [변경할 branch명]
  ```

+ 브랜치의 마지막 commit 메세지 확인

  ```bash
  $ git branch -v
  ```

## Remote Branch

### Remote Branch

+ 원격 브랜치 확인

  ```bash
  $ git branch -v
  ```

+ 로컬 브랜치를 원격 브랜치로 생성

  ```bash
  $ git push --set-upstream origin [local branch명]
  $ git push -u origin [local branch명]
  
  # git ver2.23 이상
  $ git switch -t origin/[branch명]  # 원격브랜치와 동일한 이름으로 브랜치 생성 및 변경
  ```

+ 원격 브랜치 삭제

  ```bash
  $ git push origin --delete [branch명]
  ```

+ 원격 브랜치 동기화

  ```bash
  $ git fetch --all --prune
  ```