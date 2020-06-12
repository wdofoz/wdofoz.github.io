---
title: 지킬(jekyll)에서 간단하게 상단에 스크롤 진행바 만들기 
author: wizardofoz
---

간혹 홈페이지나 기사를 보다보면 스크롤을 내릴때 상단에 100분위로 얼마나 스크롤이 진행 됬는지 표시 될 때가 있다. 물론 오른쪽에 스크롤을 보면 되지만... 상단에 표시되니 왠지 멋있어 보인다.  코드도 간단할 것 같다. 

그래서 진행바, 프로그레스바, progress bar 기타 등등으로 검색해 봤는데 이상하게도 이렇다할 마음에 드는걸 찾기가 힘들었다. 
어라 이렇게 어려운 기술일 리가 없는데???

그렇게 여러군대를 찾다찾다 보통 진행바로 검색하기보다 스크롤 진행바나 percentage progress bar 로 검색했더니 쓸만한 정보가 나왔다. 

그런데 생각보다 많이 복잡했다. 코드도 길고 스크립트도 적용해야 하고.... 그렇게 오랜시간 찾다가 간단하게 붙여 넣기만 하면 진행바가 적용되며 코드도 비교적 짧은 걸 찾아냈다. 
출처: [Adding Top Progress Bar to Websites | Webjeda](https://blog.webjeda.com/top-bar-website/#scroll-percentage-top-bar)

위치는 _layouts폴더안에 default.html 파일에서 <body> 첫부분에 다음 코드를 통째로 붙여 넣어준다. 
```
<!-- 상단 진행바 시작 -->
<style>
.progress-bar {
    background: linear-gradient(to right, white var(--scroll), transparent 0);
    background-repeat: no-repeat;
    width: 100%;
    position: fixed;
    top: 50px;
    left: 0;
    height: 3px;
    z-index: 9000;
}
</style>

<!-- This is the bar which shows scroll percentage -->
<div class="progress-bar"></div>


<!-- Script used to generate --scroll variable with current scroll percentage value -->
<script>
var element = document.documentElement,
  body = document.body,
  scrollTop = 'scrollTop',
  scrollHeight = 'scrollHeight',
  progress = document.querySelector('.progress-bar'),
  scroll;

document.addEventListener('scroll', function() {
  scroll = (element[scrollTop]||body[scrollTop]) / ((element[scrollHeight]||body[scrollHeight]) - element.clientHeight) * 100;
  progress.style.setProperty('--scroll', scroll + '%');
});
</script>
<!-- 하단 진행바 끝 출처: https://blog.webjeda.com/top-bar-website/#scroll-percentage-top-bar -->   
```

나는 메뉴 바로 아래부터 스크롤이 진행 되었으면 싶어서 스타일에서 top만 50px로 변경했다.  메뉴바 높이가 50px이기 때문이다. 
만약 가장 위로 하고 싶으면 top:0로  하단으로 하고 싶으면 top을 bottom으로 변경하면 된다.  