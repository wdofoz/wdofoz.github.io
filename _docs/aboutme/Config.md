---
title: 지킬(jekyll)에서 내가 원하는 스타일로 정보 바꾸기

---

지킬 독템을 구동 했으니 이제 몇가지 개념을 잡아보고 내 스타일로 정보를 수정합시다.

## 지킬의 구동 원리를 나름 정리

우리가 아는 보통의 홈페이지는
>내가 접속을 하면 서버가 일을 열심히 해서 정보를 찾아 나에게 정보를 전달해 준다.

로 알려져 있다.

여기서 내가 이해하는 개념은 이렇다.

>내가 접속을 하면 서버가 일을 열심히 해서 (php), 정보를 찾아 (sql에서) 나에게 정보를 전달해 준다. (아파치나 iis 같은 서버프로그램)  
내 PC는 전달받은 정보를 가지고 css나 자바같은 걸 알아서 해석해 예쁘게 화면에 표시한다.!

꼭맞는지는 모르겠지만 대충 이러하다. php나 sql은 홈페이지의 리소스를 너무 많이 (는 아니고 쪼큼 많이?) 잡아 먹기 때문에 서버를 느리게 하는 주범이지만 요즘엔 서버가 좋아서 뭐.ㅎㅎ

깃허브는 php와 sql없이 나에게 파일만 전달해주는 서버 역할만 한다. 그럼 나머지 역할은 누가하는냐?

루비를 통해 지킬을 설치한 내 컴퓨터에서 다 연산해 주고 결과물만 전달해 준다. (이 설명이 더 어렵나???)

아무튼

서버는 html등 기타 파일 전달만 하고 관련 연산은 미리 다 해놨으니 결과만 보내주고 받은 파일로 이러쿵 저러쿵 하는건 받는 사람 브라우저에서 알아서 한다.
그러니 지킬로 블로그를 만들면 php고 sql 이고 AMP고 나발이고 다 필요 없이 아파치나 iis나 Nginx 같은 서버 구동만 프로그램만 있으면 되고 거의 모든 운영체제는 이걸 모두 기본 제공하고 있다. 어떤 pc로든 쉽게 서버를 올릴 수 있다.

그래서 우리는 지킬의 결과물인 _site 폴더 안에 있는 것만 잘 올리면 블로그 구동을 할 수 있다.

물론 php와 db(sql)이 없기 때문에 실시간 덧글이나 게시판 운용은 한계가 있지만 나는 소통에 관심이 없으니 불필요한 기능이고 필요한 사람들은 가장 간단하게는 구글 설문 부터 덧글, 이메일 수신 등 각종 대안들이 있기 때문에 찾아보면 금방 자료를 얻을 수 있다.

## 지킬 페이지 구조에 대해 슬쩍 보고 갑시다.

지킬은 내가 작성하거나 설정한 각종 값들이 _site에 결과물로 내보내기 되어진다.
그리하여 처음 구조를 이해하지 않으면 이 파일이 왜 여기 갔는지 저기 갔는지 헤깔리게 된다.
몰라도 구동하는데 문제는 없지만 알면 좋지 뭐

지킬 독템 기준으로 설명하자면

```
.
├── _data               # 폴더- 페이지 구성하는데 필요한 기본 정보 파일이 저장된다.
├── _docs               # 폴더- 홈페이지의 데이터가 저장된 곳이다. (일반 사이트 DB개념이라는데.)
├── _includes           # 폴더- 반복해서 불러오기 하는 파일이 저장되는 곳이다.
├── _layouts            # 폴더- 레이아웃을 구성하는 파일들이 저장된다.
├── _posts              # 폴더- 포스트 그러니까 게시글 개념이 글들이 작성되고 보관되는 곳이다.
├── _sass               # 폴더- 부트스트랩이나 부트스와치 같은 css 파일이 담겨 있는 곳이다.
├── assets              # 폴더- 이미지, 동영상과 같은 기타 파일을 저장하는 곳이다.
├── .editorconfig       # 모르는 놈이라 그냥 뒀다.
├── .gitignore          # 없어질 놈이라 그냥 뒀다.
├── _config             # 홈페이지에 대한 기본 설정을 저장하는 곳
├── Dockerfile          # 도커 파일인데 그냥 뒀다.
├── favicon             # 브라우저 상단에 코딱지 만한 로고... 자신 스타일로 바꿔줘라.
├── Gemfile             # 없어질 놈이라 그냥 뒀다.
├── Gemfile.lock        # 없어질 놈이라 그냥 뒀다.
├── index               # 매인 페이지라 잘모셔 두었다.
├── LICENSE             # 라이선스 파일이라 그냥 두었다.
├── nginx.conf          # 서버 설정인데 그냥 두었다.
└── README              # 없어질 놈이라 그냥 뒀다.

```

지킬에서 마크다운 (MD)파일이나 (html) 파일은 모두 html 파일로 변환되어 생성 된다.
폴더나 파일 앞에 언더바 (_)가 들어가는 것들은 사이트 생성시 자동 제외 된다. 따라서 안에 있는 파일들은 _config.yml에 있는 설정에 의해 자동으로 별도 폴더로 들어가 생성 된다.  위의 구조로는 assets를 제외한 모든 폴더는 없어진다고 보면 된다.


## 그럼 이제 부터 게임을 시작하지..._config.yml

페이지는 이미 올랐고 이제 페이지를 나에게 맞게 수정할 일만 남았다.

기본 설정은 지난번 수정한 이후 이렇게 된다.

``` yml
# Site settings
title: Jekyll Doc Theme
email: your-email@domain.com
description: >
  Jekyll Template for Project Websites
  providing documentation and blog post pages.
lang: en-US

baseurl:  # the subpath of your site, e.g. /blog/
url:  # the base hostname & protocol for your site
git_address: https://github.com/aksakalli/jekyll-doc-theme
git_edit_address: https://github.com/aksakalli/jekyll-doc-theme/blob/gh-pages

# theme options from https://bootswatch.com/3/
# comment out this to use default Bootstrap
bootwatch: Slate # cerulean cosmo custom cyborg darkly flatly journal lumen readable sandstone simplex slate solar spacelab superhero united yeti

# Build settings
markdown: kramdown
highlighter: rouge
# Plugins (previously gems:)
plugins:
  - jekyll-feed
  - jekyll-redirect-from
  - jekyll-seo-tag
  - jekyll-sitemap

exclude:
  - Gemfile
  - Gemfile.lock
  - .idea/
  - .gitignore
  - README.md
timezone: Europe/Berlin
defaults:
- scope:
    path: _posts
    type: posts
  values:
    layout: post
    sectionid: blog

- scope:
    path: _docs
    type: docs
  values:
    layout: docs
    sectionid: docs
    seo:
      type: "WebPage"

collections:
  docs:
    permalink: /:collection/:path/
    output: true
  posts:
    permalink: /blog/:year/:month/:day/:title/
    output: true
```

참 많지만 하나하나 뜯어 보자.


  1. `title: Jekyll Doc Theme` 타이틀이다. 나는 WizardOfOz로 바꿔줬다.
  2. `email: your-email@domain.com` 이메일이다. 내 이메일로 바꿔줬다.
  3. `description: ` 대충 사이트 소게인것 같길레 좋아하는 가사를 적었다.
  4. `lang: en-US` 언어 설정 같길레 ko-KR로 바꿔줬다.
  5. `baseurl:` 가상 디렉토리를 사용할 경우 쓰는것 같아 비워두었다.
  6. `url: ` 홈페이지 주소가 될 것 같은 녀석을 적어 두었다.
  7. `git_address: , git_edit_address: ` 깃허브 활용을 거의 안하니 그냥 비워 두었다.
  8. `bootwatch:` 부트스와치를 활용한 테마를 선택해야 하길레 지난번에 Slate로 지정 했다. 나는 여기서 <span class="text-danger">부트스트랩</span> 이란 개념을 만나 즐거움과 동시에 밤샘 작업이 늘었다 줄었다 했다. 중요한 개념이니 [아래](#부트스트랩을-나-나름대로-이해한-방법)에서 한번 다룰거다.
  9. `markdown: kramdown, highlighter: rouge` 글쓰는 다크다운 문법을 뭘로 쓸지, 코드를 부분은 어떻게 할지 같은데 모르는 부분이라 고이고이 간직했습니다.
  10. `plugins:` 여러 플러그인들일 터인데... 이것도 고이고이 간직했습니다. 나중에 toc이란 기능만 추가했지만 이건 나중일이니...
  11. `exclude:` 지킬은 기본적으로 폴더나 파일 앞에 언더바(_)가 붙은 건 가져 오지 않는데 언더바 붙은거 외에도 안가져올 파일을 직접 지정할 수 있는 것 같습니다.
  12. `timezone:` 시간은 asia/seoul로 바꿨습니다.
  13. `defaults: - scope:` 해당 폴더에 쓴 글에 대한 기본 값을 정의 한다. 예를 들어 _docs 폴더 안에 들어간 모든 파일은 레이아웃이 docs다. 이 부분은 일단 건들지 말고 지나갑시다... 홈페이지 기본 기능에 충실한 삶을 살아 보자구요...(난 바꿨지만... 머리아파 후회했다.)
  14. `collections:` 사이트 기본 문서인 docs와 계속 게시해 나갈 게시글인 posts를 어느 폴더로 출력할 지 결정 하는 부분. 내가 건드릴 영역이 아니니 고대로 두고 지나 갑시다. (물론 난 바꿨지만 또 머리아파 후회했다.) 지금 설정은 13번에서 타입이 docs로 된 문서들은 collections이름인 docs 폴더 안에 경로를 따라 폴더를 생성하고 파일이름을 index.thml로 생성하란 의미이고  posts로 된 문서들은 사이트주소/blog/년도/월/날짜/제목/폴더 안에 index.html파일로 만들어 넣으란 이야기다.

결국 수정한 건 1-6, 그리고 8, 12 정도다.

_config.yml에 있는 파일을 수정한 뒤에는 반드시 지킬 서버를 재구동 시켜줘야 수정분이 반영된다. (엄청 귀찮....)

## 부트스트랩을 나 나름대로 이해한 방법

지킬을 보면서 부트스트랩이란 개념을 처음 접하게 되었는데 나같은 코드 무지랭이 들에게는 매우매우매우 기쁜 소식임과 동시에 최근 생성되는 웹페이지는 거의 기본으로 깔고 간다고 한다.   
일단 내가 이해한 개념은 이렇다.

수천만개의 웹페이지가 있지만 사실 사람들이 쓰는 디자인(css) 및 기능(자바스크립트)은 다 고기서고기인 부분이 많이 있다. 내가 생각한 건 누군가 이미 생각했을 가능성이 매우 높다. 그래서 사람들은 이러한 공통된 것들을 **미리** 만들어 놓고 같은 클래스나 아이디 이름으로 사용하도록 **약속** 해 놨으며 그걸 모아둔 것이 <span class="text-danger">부트스트랩</span>이란 프로젝트로 이해 했다. <http://bootstrapk.com/>

따라서
>내가 직접 css나 자바스크립트를 작성하지 않고 누가 만들어 놓은 걸 클래스 네임을 추가 하는것 만으로 사용할 수 있는 웹디자인 도구

라고 이해 했는데 꼭 맞는지는 모르겠다.

여기에 부트스와치는 부트스트랩을 기반으로 디자인 적인 요소를 아에 테마로 지정해 놓은 개별 웹디자인 테마 프로젝트다.<https://bootswatch.com/>   
웹페이지 문구가 Free themes for Bootstrap 인것만 봐도 성격을알 수 있다. 다만 지킬독템에서만 쓰는지 다른곳에서도 사용하는 개념인지는 잘 모르겠다. 홈페이지 들어가서 마음에 드는 테마를 골라서 _config.yml파일의 bootswatch 항복을 변경하면 테마(색)이 순식간에 바뀐다.
