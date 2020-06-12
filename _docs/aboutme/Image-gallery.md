---
title: 지킬(jekyll)에서 간단하게 이미지 갤러리 만들기 
author: wizardofoz
---

# 지킬 블로그에서 갤러리 기능 사용하기
지킬 블로그를 만들면서 제일 귀찮은 것 중 하나는 이미지를 첨부하는 일이다. 
워드프레스나 기타 html 에디터에서는 이미지를 첨부하는 것 만으로도 자동 업로드가 되는데 지킬은 이미지를 넣어주고 넣은 이미지를 다시 링크 해야 하는 귀찮고도 귀찬은 과정을 거쳐야 한다. 간혹 넣는 사진도 이모양인데 사진 열댓개를 한번에 올린다면 얼마나 귀찮을 까???
그래서 갤러리 기능을 찾아 봤는데 루비를 추가로 넣어야 하는 귀찮은 과정이 있었다. (더 편한가?? 암튼 난 익숙치 않음) 내가 원하는 건 이미지를 폴더 안에 넣어두면 지가 알아서 폴더에 있는 이미지를 통째로 읽어 드리는 기능이다. 그러면 궅이 내가 이미지 추가 코드를 작성하지 않아도 되지 않겠어???(더욱 격렬하게 아무것도 안하고 싶다.)

구글링에서 jekyll image auto gallery로 검색해서 나온 결과중 쓸만한건 다음 페이지 였다. 
  *  [Image gallery | Jekyll Codex](https://jekyllcodex.org/without-plugin/image-gallery/) 
설명을 보면 플러그인을 설치할 필요 없이 html 파일을 _includes에 추가하고  포스트 내용에 코드 한줄 추가하는 것 만으로 해당 폴더의 모든 이미지를 읽어와서 출력해 준다. 
페이지의 설명을 그대로 가져오면  다음과 같다. 
{ %  raw  % } 
Step 1. Download the file  [image-gallery.html](https://raw.githubusercontent.com/jhvanderschee/jekyllcodex/gh-pages/_includes/image-gallery.html) 
Step 2. Save the file in the ‘_includes’ directory of your project
Step 3. Add the following line to your layout on the place where you want the image gallery to appear:
` {% include image-gallery.html folder="/uploads/album" %}`


```image-gallery.html
<style>
    .image-gallery {overflow: auto; margin-left: -1%!important;}
    .image-gallery li {float: left; display: block; margin: 0 0 1% 1%; width: 19%;}
    .image-gallery li a {text-align: center; text-decoration: none!important; color: #777;}
    .image-gallery li a span {display: block; text-overflow: ellipsis; overflow: hidden; white-space: nowrap; padding: 3px 0;}
    .image-gallery li a img {width: 100%; display: block;}
</style>

<ul class="image-gallery">{% for file in site.static_files %}{% if file.path contains include.folder %}{% if file.extname == '.jpg' or file.extname == '.jpeg' or file.extname == '.JPG' or file.extname == '.JPEG' %}{% assign filenameparts = file.path | split: "/" %}{% assign filename = filenameparts | last | replace: file.extname,"" %}<li><a href="{{ file.path | relative_url }}" title="{{ filename }}"><img src="//images.weserv.nl/?url={{ site.url | replace: 'http://','' | replace: 'https://','' }}{{ file.path | relative_url }}&w=300&h=300&output=jpg&q=50&t=square" alt="{{ filename }}" title="{{ filename }}" /><span>{{ filename }}</span></a></li>{% endif %}{% endif %}{% endfor %}</ul>

```

재미 있는 사항은 플러그인을 사용하지 않기 때문에 이미지를 섬네일로 연산하는 작업을 별도 사이트에 맡긴다는 것이다. ([https://images.weserv.nl/](https://images.weserv.nl/))  해당 사이트에서 내용을 살짝 살펴 보면 이미지를 읽어들여 리사이징 해주는 캐싱 서버인 것을 알 수 있다. 따라서 장단점이 있는데 
장점은 섬네일을 내 사이트에서 처리하는 게 아니라 부하가 적다. 
단점은 섬네일 이미지 파일이 내 사이트에서 나오는 게 아니라 관리가 애매하다는 것이다. 

그러나 우리는 그런 볶잡한 일을 알필요 있겠는가? 
그냥 _includes 폴더에 다운받은 image-gallery.html파일 넣고 포스트 본문에
` {% include image-gallery.html folder="/uploads/album" %} ` 
한줄 넣어주면 끝나니까 그렇게 하자!

{ %  endraw  % } 