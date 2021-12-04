# Repository 병합하기(커밋 로그 유지)

## 1. 2개의 Repository 병합하기(subtree)

1. 병합될 Repository(or 새로운 Repository) 위치로 이동

   ```shell
   $ cd (병합될 Repository)
   ```

   ![병합1. cd](https://user-images.githubusercontent.com/33214969/144711742-a7940275-bfea-428b-9cd7-88073476d96f.png)

2. 원격 참조 추가

   ```shell
   # git remote add <병합할 저장소 이름> <병합할 저장소 주소>
   $ git remote add pre <https://github.com/.../pre>    # example
   ```

   ![병합1. git remote](https://user-images.githubusercontent.com/33214969/144711756-ec148671-672b-496c-bcd2-ee0d3aa9908d.png)

   + `git remote -v` : remote가 정상적으로 실행되었는지 확인

3. subtree 추가

   ```shell
   # git subtree add --prefix=<생성될 폴더명> <병합할 저장소 이름> <브랜치명>
   $ git subtree add --prefix=pre testfile2-repo master
   ```

   ![병합1. git subtree](https://user-images.githubusercontent.com/33214969/144711763-1886a1a5-178c-467e-8940-a707d9547525.png)

   + `git subtree` : 하나 이상의 커밋이 있는 Repository를 다른 Repository에 추가

4. push

   ```shell
   $ git push origin master
   ```

   ![병합1. git push](https://user-images.githubusercontent.com/33214969/144711800-a9a940b7-cad5-441c-a432-095110b9826d.png)

   + 커밋 메세지는 'Add 'pre/' from commit '5112f8816941c459cdb75bc844eccfa2dcd…'로 저장됨



## 2. 2개의 Repository 병합하기(remote&fetch&merge)

+ 전체 명령어

  ```shell
  # 병합할 저장소와 유지할 저장소에 동일한 이름의 파일이 없도록 한다. (README.md 등)
  	
  $ git remote add <병합할 저장소 이름> <병합할 저장소 주소>
  $ git fetch <병합할 저장소 이름> <브랜치명>
  $ git merge --allow-unrelated-histories <병합할 저장소 이름>/<병합하고 싶은 branch 이름>
  $ git remote remove <병합할 저장소 이름>
  $ git commit -m "Merge : <병합할 저장소 이름> into <유지할 저장소 이름>"
  ```

1. 커밋 로그를 병합하기 위해 병합할 저장소를 <병합할 저장소 이름>으로 추가

   ```shell
   # git remote add <병합할 저장소 이름> <병합할 저장소 주소>
   $ git remote add pre <https://github.com/.../pre>    # example
   ```

   ![병합2. git remote](https://user-images.githubusercontent.com/33214969/144711830-9e4e6c7d-6021-4d7b-ad58-966676a6afef.png)

   + 새로운 Repository 위치에서 `git remote`로 병합할 저장소들을 추가

2. Repository에 저장된 커밋 기록들 가져오기(병합X)

   ```shell
   # git fetch <병합할 저장소 이름> <브랜치명>
   $ git fetch pre master    # example
   ```

   ![병합2. git fetch](https://user-images.githubusercontent.com/33214969/144711848-86eec9eb-36f2-479d-af8e-dea15ea1b2c3.png)

   + `git pull` = `git fetch` + `git merge` 원격 저장소의 내용과 로컬의 최신 커밋 내용이 동일하다면 바로 pull해도 되지만, 이 경우는 그렇지 않으므로 `git fetch`로 병합할 저장소의 커밋 로그를 가져와 현재 로컬 저장소의 파일들과 비교하는 단계를 거침
   + `git reser HEAD 파일이름` : 충돌하는 파일이 있거나 제외하고 싶은 파일이 있을 경우, 병합할 목록에서 제외
   + `HEAD` : 현재 작업중인 가장 최근의 커밋

3. 연관이 없는 기록들을 병합

   ```shell
   # git merge --allow-unrelated-histories <병합할 저장소 이름>/<병합하고 싶은 branch 이름>
   $ git merge --allow-unrelated-histories pre/master    # example
   ```

   ![병합2. git merge](https://user-images.githubusercontent.com/33214969/144711888-16cbdfc9-af0e-42ca-8e82-d008811fbe1b.png)

   + `git merge` : 두 저장소의 커밋 기록을 병합
   + `--allow-unrelated-historyies` : 관계 없는 커밋 기록들을 병합할 수 있게 허용하는 옵션. 커밋 기록이 달라서 push가 불가능할 때에도 이 옵션을 사용해 push함.
   + 단, 새로운 커밋은 이전의 커밋 기록과 연결되어야 함 → 서로 다른 원격 저장소를 병합할 때에는 refusing to merge unrelate histories error가 발생하거나 아예 merge가 불가능하게 됨

4. 병합된 원격 저장소 삭제

   ```shell
   # git remote remove <병합할 저장소 이름>
   $ git remote remove pre    # example
   ```

   ![병합2. git remote remove](https://user-images.githubusercontent.com/33214969/144711907-6e28a08f-fc8a-4aab-a439-b646cea26c5c.png)

5. 병합 완료된 로그를 커밋

   ```shell
   # git commit -m "Merge : <병합할 저장소 이름> into <유지할 저장소 이름>"
   ```

   ![병합2. git commit](https://user-images.githubusercontent.com/33214969/144711937-5f4a9f90-3757-4a6f-970c-1af4ccdb3ec1.png)

   + 4번까지는 HEAD에서 일어난 거라서 아직까지 `git log`로 병합된 커밋 로그를 확인할 수 없음. 커밋하고 나면 최종 커밋 로그를 확인할 수 있음.

6. 폴더 정리

7. Push

   ```shell
   $ git push origin master
   ```

   ![병합2. git push](https://user-images.githubusercontent.com/33214969/144711947-79c6f3a1-76e6-4323-93c4-7dbeb549b7a7.png)



[참고] https://heeyeonjeong.tistory.com/11
[참고] https://velog.io/@www_1216/서로-다른-두-원격-저장소-병합하기