# ---
title: 원하는 카테고리의 리스트만 뽑아내기
---

지킬에 글을 쓰다보면 메뉴에 대한 욕심이 생긴다. 
내가 쓰는글에 카테고리 분류해 놓으면 자동으로 목록을 만들고 불러오고 리스트 나오게 할 수 없을까???
내가 하는 일은 포스트를 쓰는 것 뿐이고 나머지는 알아서 해주면 좋겠다. 이런 게으름으로 잔머리를 굴려 봤다. (어차피 같은 욕구가 있는 사람이 있을 것이고 그 사람들이 올린 글을 읽고 따라하고 살짝 바꿀뿐이지만...)

## 내가 선택한 포스트가 속한 카테고리만 보고 싶은데 모든 카데고리가 다 튀어 나온다. 
{% raw %}
사이드바에 포스트의 카테고리를 불러 오고 싶었다. 
기본으로 포스트 사이드바부분은 _layouts 폴더의 posts.html 파일안에 다음부분인다.  

```
        <div class="col-md-4">
            <div class="well">
                <h4>RECENT POSTS</h4>
                <ul class="list-unstyled post-list-container">
                    {% for post in site.posts limit:10 %}
                    <li><a href="{{ post.url | relative_url }}" {% if page.title==post.title %} class="active" {% endif %}>{{ post.title }}</a></li>
                    {% endfor %}
                    <li><a href="{{ "/allposts" | relative_url }}">All posts ...</a></li>
                </ul>
            </div>
        </div>
```

해석 하자면 
- `<div class="col-md-4"> ~ </div>` - 사이드바  영역 시작 4 숫자를 변경하면 가로 폭이 변경 된다. 
- `{% for post in site.posts limit:10 %}` -  다음 설정을 포스트에서 불러와 최대 10번 반복 불러온다. (Li 부분 반복)
- `<li><a href="{{ post.url | relative_url }}" {% if page.title==post.title %} class="active" {% endif %}>{{ post.title }}</a></li>`- 불러온 포스트의 타이틀을 url과 함께 출력하고 현제 보고 있는 페이지가 리스트와 일치하면 클레스에 active를 추가해라.
- `<li><a href="{{ "/allposts" | relative_url }}">All posts ...</a></li>`li반복 끝나면 하단에 url이 /allposts 로 되어있는  all posts …링크를 만들어라 

나는 카테고리 이름과 분류 되는 리스트를 원했기 때문에 for문 이하 부분을 다음과 같이 바꾸었다. 
```
 {% for category in site.categories %}
  <span class="name"> {{ category | first }} </span> <span class="badge">{{ category | last | size }}</span>
  <ul>
    {% for post in category[1] limit:5 %}
      <li class="list-group-item"><a href="{{ post.url }}"{% if page.title==post.title %}  class="text-warning" {% endif %}>  {{ post.title | truncate:15 }}</a> </li>
    {% endfor %}
  </ul>
{% endfor %}   
```
해석하자면 
- `{% for category in site.categories %}` - 사이트의 카테고리를명을 반복해서 불러온다. 
- `<span class="name"> {{ category | first }} </span> <span class="badge">{{ category | last | size }}</span>`  - 카테고리 이름을 불러오고 카테고리에 속한 포스트 수(size)를 불러온다. 
- `{% for post in category[1] limit:5 %}` - 카테고리 안의 포스트를 최대 5개불러온다.
- `{{ post.url }}"{% if page.title==post.title %}  class="text-warning" {% endif %}>` - 불러온 포스트의 url이 포함된 타이틀을 출력한다.  리스트의 포스트가 현재 페이지의 포스트와 일치하면 클레스에 text-warning를 추가한다. 
- `{{ post.title | truncate:15 }}` 포스트 타이틀을 15글자 이후 자른다. 
- [카테고리명(최신 포스트 타이틀*5개)]가 한셋트로 카테고리가 끝날때 까지 반복한다. 

그런데 위와 같은 사이드바를 작성하면 사이트 전체 카테고리를 모두 불러온다. 글이 얼마 없으면 모르겠는데 많아지면 우짜지???

그래서 내가 속한 카테고리만 뽑아내는 법을 찾다찾다. If 문을 활용한 문법을 보고 적용해 봤다. 
내가 속한 카테고리의 글만 뽑아내는 코드는 다음과 같다. 
```
{% for category in site.categories %}
 {% if page.category == category.first %}
  <span class="name"> {{ category | first }} </span> <span class="badge">{{ category | last | size }}</span>
  <ul>
    {% for post in category[1] %}
      <li class="list-group-item"><a href="{{ post.url }}"{% if page.title==post.title %}  class="text-warning" {% endif %}>  {{ post.title | truncate:50 }}</a>  <div class="badge">{{ post.date | date: '%Y. %m. %d' }}</div></li>
    {% endfor %}
  </ul>
    {% endif %}
{% endfor %}  
```
본문 아래 추가할 요량으로 타이틀 글자를 좀 길개 뽑았다. 
`{% if page.category == category.first %} ~ {% endif %}` - if 조건에 맞다면 (만약 현제 페이지의 카테고리와 카테고리와 일치하면)실행하고 아니면 말아라. 

위와 같은 코드를 넣으면 현재보고 있는 포스트의 카테고리와 일치하는 카테고리의 포스트 타이틀만 뽑아서 출력해 준다.  포스트 아래 부분에 적용하기 위해서 타이틀 길이를 50에서 자르고 목록은 제한을 걸어두지 않았다.


## 상단메뉴바에 포스트 카테고리를 불러오기

추가로 topmenu에 코드를 삽입해서 카테고르 목록을 자동으로 읽어 들이는 코드를 작성해 보자. 내가 원하는 것은 블로그 항목에 마우스 오버하면 카테고리 목록과 포스트 수가 뜨는 것이다. 해당 코드는 이미 위에 적혀 있는부분에서 포스트 리스트를 출력하는 부분만 빠지면 되겠다. 

원문 코드... 위치는 반복적으로 불러오기 때문에 _includes 폴더 안에 topnav.html이다.  그 중 다음 부분을 찾는다. 
```
            <ul class="nav navbar-nav">
                <li {% if page.sectionid=='docs' %} class="active" {% endif %}><a href="{{ "/docs/home/" | relative_url }}">Docs</a></li>
                <li {% if page.sectionid=='blog' %} class="active" {% endif %}><a href="{{ site.posts.first.url | relative_url }}">Blog</a></li>
            </ul>
```
해석하자면 
- `<li {% if page.sectionid=='docs' %} class="active" {% endif %}><a href="{{ "/docs/home/" | relative_url }}">Docs</a></li>` : 만약 현제 페이지가 docs면 클래스에 엑티브를 추가, **Docs**를 클릭하면 `/docs/home/` 로 이동한다. 
- `<li {% if page.sectionid=='blog' %} class="active" {% endif %}><a href="{{ site.posts.first.url | relative_url }}">Blog</a></li>` : 보고있는 페이지가 blog면 활성화 클래스에 엑티브를 추가, **Blog**를 클릭하면 최신 포스트로 가라.


이 코드중 블로그(포스트)부분을 다음과 같이 수정하였다. 

```
<li><a id="catemenu" class="dropdown-toggle" href="#" role="button"  data-toggle="dropdown" aria-haspopup="true" aria-expanded="false"> Category <span class="caret"></span></a> 
 <!-- id="dropdownMenuLink" -->                 
        <ul class="dropdown-menu" aria-labelledby="catemenu" >
          {% for category in site.categories %}
          {% for post in category[1] limit:1 %}
           <li><a href="{{ post.url }}"> {{ category | first }}<span class="badge pull-right">{{ category | last | size }} </span></a> 
           </li>         {% endfor %}   
          {% endfor %}  
        </ul>
     </li>       
```
해석하자면
-   `<li><a id="catemenu" class="dropdown-toggle" href="#" role="button"  data-toggle="dropdown" aria-haspopup="true" aria-expanded="false"> Category <span class="caret"></span></a>           <ul class="dropdown-menu" aria-labelledby="catemenu" >` : 토글 (마우스 오버로 하위메뉴 생성) 클래스 및 기능을 불러온다. 기본 설치된 (bootstrip에서), ul 이하의 항목을 (li)내용을 토글로 편다. ([부트스트립 드롭다운 토글 기능 참고)](https://getbootstrap.com/docs/4.0/components/dropdowns/)  
- `{% for category in site.categories %}` : 사이트의 카테고리 항목을 반복해서 불러온다.
- `{% for post in category[1] limit:1 %}` : 카테고리내의 최신 포스트 1개씩 반복해서 불러온다. 
- `<a href="{{ post.url }}"> {{ category | first }} ~ </a>` : 불러온 카테고리를 출력하고 링크는 위에서 불러온 최신포스트 url로 한다. 
- `<span class="badge pull-right">{{ category | last | size }} </span>` : 해당 카테고리의 포스트 수량을 카운트해서 출력한다. 

위의 기능을 활용하면 포스트를 작성할 때 헤드 (머릿말) 부분에 카테고리를 추가하는 것 만으로 추가 수정 없이 메뉴에 카테고리가 자동 추가될 뿐 아니라 포스트 수가 카운트 되는 것을 볼 수 있다. 

{% endraw %}