---
title: github로 githubio 블로그 제작하기
author: Whiterangi
date: 2024-11-10 16:10:00 +0900
categories: [Blogging, Tutorial]
tags: [writing]
pin: true
render_with_liquid: false
---

github를 이용하여 레포지토리명.github.io 형식의 블로그를 만드는 포스팅입니다.
테마는 chirpy를 사용했습니다.

<hr>

# 0단계: 사전 준비
1. Github 가입
2. [Visual Studio code](https://code.visualstudio.com/) 설치
3. Git 설치 (폴더에서 우클릭 > 추가 옵션 표기> 'Git bash here'가 나오는 지 확인)

# 1 단계: 깃허브 레포지토리 생성
1. 자신의 깃허브에서 ```githubusername.github.io```라는 이름의 레포지토리를 생성합니다. 저의 경우 아이디가 Whiterangi이므로 Whiterangi.github.io라는 이름의 레포지토리를 생성하였습니다.
   - 자신의 깃허브 아이디는 깃허브 profile에서 확인 가능합니다. (메인 사진 밑의 굵은 글씨 밑의 글씨)
   - 예를 들어 ```https://github.com/Whiterangi``` 저의 깃허브에 들어가보시면 Jeong Yoon Sun 밑에 Whiterangi라고 적혀있는 것을 보실 수 있습니다. 
2. 레포지토리 생성 시 public으로 생성하며, Add a README file을 체크합니다.
3. 생성한 레포지토리로 이동 후 상단의 Settings를 클릭합니다.
4. Build and deployment에서 Deploy from a branch를 선택합니다. 
5. 사이트 주소는 ```https://githubusername.github.io```가 됩니다. 

# 2단계: VScode 이용
1. 만들어진 레포지토리의 ```code``` 버튼을 클릭한 후 레포지토리 주소를 복사합니다.  
2. Visual Studio Code에 들어갑니다. 
3. 검색창에 ```>git: clone```을 검색한 후 복사한 레포지토리 주소를 붙여넣습니다. 
4. 클론할 주소를 입력하고 클론을 완료합니다.

# 로컬 확인
1. 클론한 레포지토리의 ```README.md``` 파일을 확인합니다.
2. 레포지토리에 ```index.html```을 생성합니다.
3. ```index.html```에 아래의 코드를 추가합니다.
   ```
    <html>
	  <body>
		  Hello! This is the first page!
	  </body>
    </html>
   ```
4. 바뀐 내용을 git add, commit, push 합니다.

## git add, commit, push 방법
### vscode 활용
1. vscode의 좌측 source control 에 들어간다.
2. Changes에서 변경 사항을 +한다.
3. commit 내용을 쓰고 ```commit``` 버튼을 누른다.
4. push를 완료한다.

vscode에서 push가 어렵다면 git bash를 이용해도 좋습니다.
### git bash 활용
1. 레포지토리에서 우클릭 > 추가옵션 표기> ```git bash here```
2. 아래 코드를 하나씩 입력한다.
```
git add . 
git commit -m'커밋 메시지'
git push
```
- add 시에 ```git add .```을 하게 되면 모든 변경 사항이 add 됩니다. 만약 이를 원하지 않을 시에는 ```git add 수정 파일명``` 과 같이 add 하면 됩니다.


<hr>

# 3단계: Ruby를 이용하여 local 구축
1. [Ruby 공식 사이트](https://rubyinstaller.org/)에서 Ruby + Devkit 최신 버전을 다운로드하고 설치합니다.
2. 설치 후 시작 탭에서 **Start Command Prompt with Ruby**를 실행합니다.
3. cd 명령어를 이용하여 레포지토리 주소로 이동합니다. 폴더 주소를 모를 때에는 폴더에서 ```마우스 우클릭 > 경로로 복사```를 하면 자동으로 주소가 복사됩니다. 
   ```cd C:\Users\USER\Desktop\Whiterangi.github.io```
4. 다음 명령어를 사용하여 Jekyll, Bundler, webrick을 설치합니다.
   ```
   gem install jekyll bundler
   gem install webrick
   ```
5. 설치가 완료되었는지 확인합니다:
   ```
   ruby -v
   jekyll -v
   bundler -v
   ```
6. Jekyll 프로젝트를 생성합니다.
   ```
   jekyll new ./
      # 또는
   jekyll new ./ --force
   ```
7. 종속성을 설치합니다.
   ```
   bundle install
   ```
8. 아래 코드로 Jekyll 서버를 실행하고 ```http://127.0.0.1:4000/``` 또는 ```http://localhost:4000/```에서 접속을 확인합니다.
   ```
   bundle exec jekyll serve
   ```

<hr>

# 4단계: Jekyll 테마 적용

1. 테마 선택
[여기](http://jekyllthemes.org)에서 원하는 테마를 선택할 수 있습니다. 이 예시에서는 Chirpy 테마를 사용했습니다.

2. [Chirpy GitHub 리포지토리](https://github.com/cotes2020/jekyll-theme-chirpy)에서 zip 파일로 다운로드하여 압축을 해제한 후, 로컬 리포지토리에 복사합니다.
3. 종속성을 다시 설치합니다.
   ```
   bundle install
   ```
4. 아래 코드로 Jekyll 서버를 실행하고 ```http://127.0.0.1:4000/``` 또는 ```http://localhost:4000/```에서 접속을 확인합니다.
   ```
   bundle exec jekyll serve
   ```

<hr>

# 5단계: GitHub Actions 설정

1. GitHub 레포지토리의 **Settings**에서 **Pages** > **Build and deployment** > **Source**를 **GitHub Actions**로 설정합니다.
2. **Configure** 버튼을 클릭합니다.
3. ```commit changes...```를 클릭합니다.
4. 변경사항을 커밋하고, 로컬 리포지토리에서 pull 합니다.

<hr>

# 6단계: Node.js 설치 및 빌드
1. [Node.js 공식 사이트](https://nodejs.org/)에서 최신 버전을 다운로드하고 설치합니다.

2. Ruby 프롬프트에서 다음 명령어를 실행하여 빌드를 완료합니다.
   ```
   npm install && npm run build
   ```

<hr>

# 7단계: 최종 테마 설정 및 커밋

1. `.gitignore` 파일 하단에 다음 내용을 수정합니다:
   ```
   # Misc
   # _sass/dist
   # assets/js/dist
   ```
2. `_config.yml` 파일을 열고 필요한 정보를 수정합니다:
   ```
   timezone: Asia/Seoul
   url: "https://username.github.io"
   github:
     username: username
   ```
3. 모든 변경사항을 커밋하고 푸시합니다. 이후 사이트에 변경사항이 반영되었는지 확인합니다.

<hr>

## npm error code 3221225477
저의 경우 6단계의 npm install && npm run build에서 오류가 발생했었습니다. 해당 오류 코드가 발생 시 chatgpt에게 물어보아도 Husky 파일을 수정하라는 말만 되풀이하게 됩니다.

**해결방법**
- 해당 오류가 발생 시 파일의 경로를 **모두 영어**로 바꾸면 오류가 해결됩니다. 
예를 들어,```C:\Users\예시\Whiterangi.github.io``` 이런 식으로 경로에 한국어가 있으면 오류가 발생할 수 있습니다. 이 경우 ```C:\Users\example\Whiterangi.github.io``` 이런 식으로 영어 경로로 바꿔주면 오류가 해결되게 됩니다.