---
title: 지킬(jekyll)에서 메뉴 (네비게이션) 변경

---

메인 페이지를 모조리 바꿨지만 메뉴를 못바꿨다.

메뉴를 바꾸려면 일단 jekyll doc theme 의 사이트 구조를 파악해 보자.

## 지킬의 사이트 구조

한국에서 쓰는 사이트 구조와 일반적인 블로그 사이트 구조가 좀 다른데 이는 우리나라에 네이버 카페나 다음 카페처럼 커뮤니티 카페가 잘 되어 있어서 그렇다.   
일반 커뮤니티 카페를 개념에 잡고 사이트를 생각했던 나는 여기사 한참을 헤맸는데... 지킬을 포함한 대부분의 블로그 사이트들은 다음과 같은 구조를 가진다.
  - 문서 - 사이트의 기본 정보 제공 페이지 ex) 회사소개, 연역, 사이트 설립 목적 처럼 사이트에서 잘 바꾸지 않는 항목들
  - 포스트 - 글을 써서 매번 업데이트 하는 항목
    - 카테고리: 포스트내용의 성격을 정의한다. (중복입력 가능)
    - 테그: 포스트 내용의 키워드를 입력한다. (중복입력 가능)

우리나라의 포스트 대신 게시판을 사용하는데 의외로 개인 페이지에서 게시판이 활성화 되는 곳이 우리나라 밖에 없어서 놀랐다. 대부분 개인 블로그는 포스트 형식으로 만들어 졌다.

따라서 지킬독템의 메뉴는 Docs(문서)와 Blog(포스트) 2가지 파트로 나뉜다.  
앞으로 메뉴를 수정해도 이 두가지 개념을 분리해서 생각하는 것이 편하다.



## 지킬 메뉴 바꾸기
{% raw %}
지킬독템의 메뉴는 반복해서 불러오는 항목이기 때문에 _includes 폴더 안에 topnav.html을 수정해야 한다.
브라킷으로 해당 파일을 열면 다음과 같이 나온다.
``` html
<nav class="navbar navbar-default navbar-fixed-top">
    <div class="container navbar-container">
        <div class="navbar-header">
            <button type="button" class="navbar-toggle collapsed" data-toggle="collapse" data-target="#navbar" aria-expanded="false" aria-controls="navbar">
        <span class="sr-only">Toggle navigation</span>
        <span class="icon-bar"></span>
        <span class="icon-bar"></span>
        <span class="icon-bar"></span>
      </button>
            <a class="navbar-brand" href="{{ site.baseurl }}/">
                <span><img src="{{ "/assets/img/logonav.png" | relative_url }}" alt="Logo"></span> {{ site.title }}
            </a>
        </div>
        <div id="navbar" class="collapse navbar-collapse">
            <ul class="nav navbar-nav">
                <li {% if page.sectionid=='docs' %} class="active" {% endif %}><a href="{{ "/docs/home/" | relative_url }}">Docs</a></li>
                <li {% if page.sectionid=='blog' %} class="active" {% endif %}><a href="{{ site.posts.first.url | relative_url }}">Blog</a></li>
            </ul>
            <div class="navbar-right">
                <form class="navbar-form navbar-left">
                    <div class="form-group has-feedback">
                        <input id="search-box" type="search" class="form-control" placeholder="Search...">
                        <i class="fa fa-search form-control-feedback"></i>
                    </div>
                </form>
                <ul class="nav navbar-nav">
                    <li><a href="{{ site.git_address }}"><i class="fa fa-github" aria-hidden="true"></i></a></li>
                </ul>
            </div>
        </div>
    </div>
</nav>
```

여기서 Docs와 Blog 를 원하는 이름으로 변경해 주거나 한줄을 통째로 복사해 새로운 항목으로 추가해 줄 수 있다.
항목을 설명하자면

  - 스마트폰에서 볼때 나타나는 토글 버튼
    ``` html
      <button type="button" class="navbar-toggle collapsed" data-toggle="collapse" data-target="#navbar" aria-expanded="false" aria-controls="navbar">
        <span class="sr-only">Toggle navigation</span>
        <span class="icon-bar"></span>
        <span class="icon-bar"></span>
        <span class="icon-bar"></span>
      </button>
    ```
  - 메뉴 제일앞에 있는 로고와 사이트명

    ``` html
      <a class="navbar-brand" href="{{ site.baseurl }}/">
        <span><img src="{{ "/assets/img/logonav.png" | relative_url }}" alt="Logo"></span> {{ site.title }}
      </a>
    ```

  - 메뉴 이름 변경 할 수 있는 항목
    1. `li` 메뉴 항목 추가
    2. `{% if page.sectionid=='docs' %} class="active" {% endif %}`만약 페이지 섹션이름이 docs라면 클래스에 엑티브를 추가
    3. `<a href="{{ "/docs/home/" | relative_url }}">Docs</a>` 항목 이름은 Docs로 하고 클릭하면 홈페이지/docs/home/로 가라
  - 메뉴의 검색바
    ```html
    <div class="form-group has-feedback">
        <input id="search-box" type="search" class="form-control" placeholder="Search...">
        <i class="fa fa-search form-control-feedback"></i>
    </div>
    ```
  - 깃허브로 가는 링크
    ```html
    <ul class="nav navbar-nav">
        <li><a href="{{ site.git_address }}"><i class="fa fa-github" aria-hidden="true"></i></a></li>
    </ul>
    ```

위의 내용을 참고해서 항목을 추가하거나 제거 해주면 된지만 잘 모르겠다면 **메뉴 이름 변경 항목**에서 docs와 blog정도 바꿔 주는걸 추천한다.

나 같은 경우 메뉴바에 Allpost 항목과 카테고리 항목을 추가 했는데 이는 카테고리에 대한 이해가 필요해서 이후에 다룰 [카테고리](/docs/aboutme/category) 항목에서 다시 다루려고 한다.
{% endraw %}
