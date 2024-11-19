---
title: chirpy github 블로그 검색 노출하기
author: Whiterangi
date: 2024-11-17 11:52:00 +0900
categories: [Blogging, Tutorial]
tags: [writing]
pin: true
render_with_liquid: false
---
이전 포스팅까지 하여 블로그를 만들고, 댓글과 조회수 기능을 추가하였습니다. 이번 포스팅에서는 내 블로그를 구글과 네이버에 노출시키는 방법에 대해 알아보도록 하겠습니다.

# 1. Google 검색 노출
## 1단계:  Google Search Console
1. [Google Search Console](https://search.google.com/search-console/about)에 접속 후 로그인합니다.
![start](https://github.com/Whiterangi/Whiterangi.github.io/blob/main/assets/img/blog%20img/2024-11-17-search-exposurey/1.png?raw=true)
2. ```URL 접두어```에 내 블로그 주소를 등록합니다.
![URL_link](https://github.com/Whiterangi/Whiterangi.github.io/blob/main/assets/img/blog%20img/2024-11-17-search-exposurey/2.png?raw=true)

## 2단계: 소유권 확인하기
1. 소유권 확인 > 다른 확인 방법 > ```HTML 태그``` 를 누릅니다.
2. ```1.```에 나오는 코드를 복사해줍니다. (확인 버튼은 아직 누르면 안 됩니다.)
![meta_code](https://github.com/Whiterangi/Whiterangi.github.io/blob/main/assets/img/blog%20img/2024-11-17-search-exposurey/3.png?raw=true)
3. ```_config.yml``` 파일에 들어갑니다.
4. 복사한 코드의 ```<<meta name="google-site-verification" content="어쩌고저쩌고">```에서 ```어쩌고저쩌고``` 부분을 ```_config.yml```코드에 추가합니다.
```
# Site Verification Settings
webmaster_verifications:
  google: 어쩌고 저쩌고 # fill in your Google verification code
  bing: # fill in your Bing verification code
  alexa: # fill in your Alexa verification code
  yandex: # fill in your Yandex verification code
  baidu: # fill in your Baidu verification code
  facebook: # fill in your Facebook verification code
```
1. git add, commit, push 후에 google search console 사이트로 돌아가 확인 버튼을 누릅니다.

## 3단계: 사이트맵 제출하기
1. 소유권 확인 완료 후 속성으로 이동합니다.
2. 속성에서 > ```Sitemaps```에 들어갑니다.
![Sitemaps1](https://github.com/Whiterangi/Whiterangi.github.io/blob/main/assets/img/blog%20img/2024-11-17-search-exposurey/4.png?raw=true)
3. ```새 사이트맵 추가``` > ```사이트맵 URL 입력``` 에서 ```sitemap.xml```를 적어줍니다.
![Sitemaps2](https://github.com/Whiterangi/Whiterangi.github.io/blob/main/assets/img/blog%20img/2024-11-17-search-exposurey/5.png?raw=true)
4. 사이트맵이 제출되면 구글 검색 노출은 완료됩니다. 

# 2. Naver 검색 노출
## 1단계: Naver Search Advisor
1. [네이버 서치 어드바이저](https://searchadvisor.naver.com/)에 들어갑니다.
2. Naver 아이디로 로그인을 진행합니다.
3. 페이지 중간의 ```웹마스터 도구 사용하기```을 클릭합니다.
![Naver_Wepmaster](https://github.com/Whiterangi/Whiterangi.github.io/blob/main/assets/img/blog%20img/2024-11-17-search-exposurey/6.png?raw=true)
4. 사이트 등록에 자신의 블로그 주소를 넣습니다.
![my_site](https://github.com/Whiterangi/Whiterangi.github.io/blob/main/assets/img/blog%20img/2024-11-17-search-exposurey/7.png?raw=true)

## 2단계: 소유권 확인
1. 사이트 소유 확인에서 ```HTML 태그```를 선택하고 메타 태그를 복사합니다.
![HTML_tag](https://github.com/Whiterangi/Whiterangi.github.io/blob/main/assets/img/blog%20img/2024-11-17-search-exposurey/8.png?raw=true)
2. ```includes/head.html```에 들어갑니다.
3. ```head.html```에 복사한 메타 태그를 붙여넣기 합니다. (이때 위치는 크게 중요하지 않습니다.)
4. git add, commit, push 합니다.

## 3단계: 사이트맵 제출하기
1. 요청 > 사이트맵 제출 > 사이트맵 URL 입력에서 ```자신의 블로그 주소/sitemap.xml```을 입력하고 확인 버튼을 누릅니다.
2. 제출된 사이트맵을 체크합니다.
![Sitemaps3](https://github.com/Whiterangi/Whiterangi.github.io/blob/main/assets/img/blog%20img/2024-11-17-search-exposurey/9.png?raw=true)
3. 사이트맵이 제출되면 네이버 블로그에서 자신의 블로그가 검색됩니다.