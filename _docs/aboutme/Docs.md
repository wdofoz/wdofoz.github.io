---
title: 지킬(jekyll)에서 문서와 포스트를 수정해 봅시다.
---

블로그엔 문서 페이지 (기본 페이지)와 글을 올리는 포스트로 나눈다.  
먼저 사이트의 기본이 되는 문서 부분을 수정하고 다음 포스트를 입력해 보자.
## 문서 페이지 확인하기
메뉴에 Docs를 클릭하면 문서 페이지로 이동한다.  
나오는 페이지의 좌측에 사이드메뉴(네비게이션)를 보면 Getting Started 과 Examples 두 토글바(접히는 메뉴)를 볼 수있다.  
이 메뉴 항목은 테마 폴더의 _data/docs.yml 파일에 내용을 수정하면 바꿀 수 있다.
_data 폴더는 다른 사이트로 말하면 '데이터 베이스'를 보관하는 곳이라고 생각하면 된다.  
docs.yml의 내용을 보면 다음과 같다.
```
  - title: Getting Started
    docs:
    - home
    - themes
    - customization

  - title: Examples
    docs:
    - cheatsheet
    - font-awesome
    - bootstrap
```
위의 내용을 바탕으로 사이드 바에 대해 몇가지 설명하자면
  1. title 항목을 추가로 만들면 토글바가 추가로 자동 생성 된다.  
  2. 토글바를 클릭해서 생기는 항목들은 docs 항목에 있는 메뉴들이 있는데 이 숫자는 docs 항목의 숫자 만큼 칸이 생긴다.
  2-1 docs.yml 안에 docs 아래 있는 이름의 문서가 url로 존재 해야만 페이지가 생성 된다. ex) docs.yml의 home이 활성화 되려면 페이지 문서 url이 exhome.com/docs/home 로 존재 해야만 해당 페이지의 문서 제목이 항목에 활성화 된다.(정 모르겠다 싶으면 머릿글에 permalink로 경로를 강제적으로 잡아줘라)
  3. 활성화 된 페이지의 토글바은 펼쳐지고 그 외의 토글바는 모두 접힌 상태로 설정된다.
  4. 현재 열린 페이지와 같은 항목은 클레스에 active 가 추가되어 다른 항목이랑 다른 색을 가지게 된다.

## 문서 작성법
docs 안에 들어갈 문서들은 마크 다운을 기본 문법으로 사용한다.  
지킬에서 마크다운 문서나 포스트 작성시 꼭 들어가야 하는 YAML머릿말 (Front Matter)이 있다. 형식은 다음과 같다.
  ```
  ---
  layout: docs
  title: Welcome
  permalink: /docs/home/
  redirect_from: /docs/index.html
  tags: [i, love, you]
  ---
  ```
- title은 문서의 제목이기 때문에 꼭 포함 되어야 한다. (나는 대부분의 문서에 title만 입력하도록 설정했다.)
- layout, permalink는 입력하지 않을 경우 테마폴더의 _config.yml 파일 안에 들어 있는 설정을 따른다.
이 YAML머릿말 (Front Matter)의 우선순위가 가장 높기 때문에 사전에 설정한 모든 설정값 보다 가장 우선되어 적용 된다. permalink는 해당 문서의 사이트 경로가 된다.
- redirect_from: /docs/index.html는 exgome.com/docs로 경로가 지정 되면 이 문서로 리다이렉트 하라는 의미다.
- tage: post나 문서의 키워드를 정해서 검색이나 태그가 포함된 문서 불러오기 등을 할 수 있지만 지킬독템에서는 현재 둘다 지원하지 않고 있다. 

머릿말 아래부터 내가 원하는 글을 입력하면 되는데 마크다운 기본 문법은 docs 문서페이지의 Examples 항목에 Markdown Cheatsheet 예제로 잘 나와 있다. 해당 문서는 마크다운 입력 방법이 익숙해 질 때까지 삭제하지 말고 필요한 기능 있을 시 따라하면서 익히자.

문서가 많아지면 메뉴에 문서페이지의 url을 연결해 메뉴 항목을 늘리는 방법으로 하는 것이 좋다. (나는 별도 폴더에 넣었다가 고생이 이만저만이 아니였으니 나같은 뻘짓은 안하시길 추천드립니다.)


## 포스트 작성법
포스트는 _post폴더 안에 들어가면 되는데 파일 형식을 지켜주이 않으면 html이 생성 되지 않은다. 형식은 다음과 같다. 
YYYY-MM-DD-Posttitle.md (예: 2020-03-14-docsnpost.me)

머릿말 작성은 docs와 같지만 케테고리 기능은 post만 지원하는 것 같다. (확실치는 않지만 나는 포스트 외의 문서에서 카테고리 불러오기를 성공하지 못했다.)
  ```
  ---
  layout: post
  title: 나의 처음 포스트
  tags: [i, love, you]
  category: post
  permalink: /blog/first/
  author: wizardofoz
  ---
  ```
  - title은 문서의 제목이기 때문에 꼭 포함 되어야 한다. (나는 대부분의 포스트에서 title과 카테고리, author(작성자)만 3개만 입력하도록 설정해 두었다.)
  - `layout, permalink`는 설정하지 않으면 _config.yml 파일 안에 있는 설정으로 적용된다. 
  - `jekyll doc theme`의 경우 permalink를 입력하지 않을 경우 _config.yml의 기본 경로를 예로 들면 http://myweb.com/blog/YYYY/MM/DD?Posttitle/index.html이다.  [지난 문서에서 확인](https://wdofoz.qhop.org/docs/aboutme/Config/#%EA%B7%B8%EB%9F%BC-%EC%9D%B4%EC%A0%9C-%EB%B6%80%ED%84%B0-%EA%B2%8C%EC%9E%84%EC%9D%84-%EC%8B%9C%EC%9E%91%ED%95%98%EC%A7%80_configyml)
  - `category`: 포스트의 글의 성격이 어떻게 되는지 항목화 할 수 있다. 
  - `tage`: post나 문서의 키워드를 정해서 검색이나 태그가 포함된 문서 불러오기 등을 할 수 있지만 지킬독템에서는 현재 둘다 지원하지 않는것 같아 추가로 기능을 넣어 주어야 한다. 
  - `author`: 포스트 글의 작성자.
  
포스트의 경우 docs 처럼 정적인 문서가 아니기 때문에 양이 많아서 카테고리로 쉽고 빠르게 분류 할 수 있게 만들어 놓은 것 같다. 문서 작성시 예제 페이지의 마크다운 예시를 참고 하면 좋다. 
  
  
  