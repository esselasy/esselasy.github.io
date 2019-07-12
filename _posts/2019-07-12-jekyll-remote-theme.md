---
title: "Jekyll Minimal Mistake 테마 사용법"
date: 2019-07-12 22:00:00 +0900
tags: jekyll github
---
[Jekyll Minimal Mistake](https://mmistakes.github.io/minimal-mistakes/) 테마로 
Github Pages를 만드는 방법은 아래 글을 참고하였다.

[쉽고 빠르게 수준 급의 GitHub 블로그 만들기 - jekyll remote theme으로](https://dreamgonfly.github.io/2018/01/27/jekyll-remote-theme.html)

이후에 수정한 내용을 간단하게 정리한다.

# _config.yml
## 테마 설정
````
remote_theme             : "mmistakes/minimal-mistakes"
minimal_mistakes_skin    : "default" # "air", "aqua", "contrast", "dark", "dirt", "neon", "mint", "plum", "sunrise"
````

## 사이트 설정
````
locale                   : "ko-KR"
title                    : # 사이트 최상단에 표시되는 타이틀
name                     : # 왼쪽에 표시되는 저자명
````

## 저자
````
# Site Author
author:
  name             : # 저자명
  avatar           : # 프로필 사진 경로 e.g. "/assets/images/photo.jpg"
  bio              : # 저자명 아래에 표시되는 소개글
  location         : # 지역
  links:             # 깃허브 주소만 추가
    - label: "GitHub"
      icon: "fab fa-fw fa-github"
      url: "https://github.com/esselasy/"  
````

## Archive 설정
### 카테고리 설정
[_pages/category-archive.md](https://github.com/mmistakes/minimal-mistakes/blob/master/docs/_pages/category-archive.md)를 /categories/index.html로 복사한다.

### 태그 설정
[_pages/tag-archive.md](https://github.com/mmistakes/minimal-mistakes/blob/master/docs/_pages/tag-archive.md)를 /tags/index.html로 복사한다.

### 작성 글에서 카테고리와 태그 사용법
````
---
title: "Unity Test Runner"
categories: Unity
tags: Test 번역
---
````
