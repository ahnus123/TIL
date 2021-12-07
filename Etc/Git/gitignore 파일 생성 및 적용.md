# .gitignore 파일 생성 및 적용

1. .gitignore 파일 생성 및 적용

   ## **.gitignore 파일**

   ```tex
   ▫️ 커밋 시 무시할 파일 or 디렉토리를 설정하는 파일
   ```

   ## **.gitignore 파일 생성 (mac)**

   1. .gitignore 파일 생성

      ```bash
      # 프로젝트 위치로 이동
      $ cd (프로젝트 위치)
      
      # .gitignore 파일 생성
      $ sudo touch .gitignore
      
      # Finder에서 해당 파일이 안보이는 경우
      # Shift + command + .
      ```

   2. .gitignore 파일 작성

      + gitignore 자동 생성 사이트 : [gitignore.io](https://www.toptal.com/developers/gitignore)

      + 해당하는 운영체제, 개발환경(IDE), 언어 등을 선택하면 그에 맞는 gitignore 파일 내용을 출력해줌

        <img src="https://user-images.githubusercontent.com/33214969/145714098-19bbc735-73ab-4476-a051-f0deafba128d.png" alt="gitignore1.png" style="zoom:33%;" />

      + 해당 내용을 .gitignore 파일에 복사

        + 만약 .gitignore 파일 수정할 때 "사용자가 '.gitignore' 파일을 소유하지 않고 있으며, 이에 대한 쓰기 권한이 없습니다."라는 팝업 발생 시, Finder → .gitignore 파일 우클릭 → '정보 가져오기'에서 권한 확인
        + 만약 권한에 자신이 등록되어 있지 않은 경우, 우측 하단 자물쇠 풀기 → '+' 버튼으로 나 자신을 추가 → '읽기 및 쓰기' 권한 부여

   3. 해당 repository에 적용

      + 해당 repository에 .gitignore 파일 내용 적용

        ```bash
        # Optional (이전에 작업한 내용들에도 .gitignore 적용할 때)
        $ sudo git rm -r --cached .    # git 트래킹 풀어주기
        $ sudo git add .    # git commit할 파일 대상 정렬
        
        # 필수
        $ sudo git commit -m ".gitignore 생성"    # commit
        $ sudo git push origin master    # push
        ```