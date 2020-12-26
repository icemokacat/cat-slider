# cat-slider
간단한 이미지 슬라이더 입니다.

PC 에서는 좌우 버튼을 클릭하여 슬라이더를 이동시키고

모바일에서 스와이프를 통해 슬라이드를 이동시킬 수 있습니다.

예제 : https://icemokacat.github.io/cat-slider/

해당 플러그인을 사용하기 위해서 아래의 js가 필요합니다.

1. jquery

2. jQuery Mobile (v1.5.0-alpha.1) https://jquerymobile.com/

   > jQuery mobile 용량이 커서 custom builder 에서 Events > Touch 만 선택 후 다운 받으시면 작은 용량으로 사용할 수 있습니다.
   >
   > 해당 js 호환성이 깨질 경우 touchstart , touchend , touchmove를 직접 구현하셔야 합니다.

최대한 CSS 를 적게 쓰면서 슬라이더 플러그인을 customizing 하기 쉽도록 구현하였습니다.
진행하는 프로젝트의 css 및 레이아웃에 맞춰 customizing 하시려면 min 버전이 아닌 full 버전을 받아서 직접 수정하시면 됩니다.

예제 사용법
1. 창의 크기를 보고 싶은 정도로 설정합니다. (브라우저 창)
2. 이미지 width 를 고른 후 draw 버튼 클릭
3. SLIDE SET 버튼을 클릭
	> swipe 기능은 링크 진입 후 개발자 도구 (F12) 를 여신 후 
	> 개발자 도구 좌측 상단의 2번째 아이콘인 "Toggle device tool bar" (Ctrl + Shift + M)
	> 모바일 미리보기를 활성화 시킨 후 1,2,3 순서대로 진행하시면 됩니다.
	> 아직 destroy 문제가 해결되지 않아서 다른 이미지로 보시려면 새로고침 후 크기를 다시 고른 후 1,2,3을 진행하셔야 합니다.


# 1. 사용법

### 1.1 레이아웃 구성

구성해야할 요소는 액자,슬라이더판,슬라이더 입니다.
* 클래스명은 자유롭게 작성하시면 됩니다.	

```html
<div class="slider-wrapper">
</div>
```

먼저 슬라이드 wrapper(액자)를 구성해줍니다. 
![enter image description here](https://raw.githubusercontent.com/icemokacat/cat-slider/main/doc/img/wrapper.png)

다음 슬라이드판을 구성합니다.
슬라이더는 아래 구성한 슬라이더판이 액자안에서 움직이면서 슬라이딩 됩니다.
요소는 div , ul 등 아무거나 작성하셔도 됩니다.
이번 예제에서는 ul > li 로 구성합니다.

```html
<div class="slider-wrapper">
    <ul class="cat-slider">
    </ul>
</div>
```
![enter image description here](https://raw.githubusercontent.com/icemokacat/cat-slider/main/doc/img/sliders.png)

마지막으로 슬라이더 판에 슬라이더 이미지를 채워 줍니다.

   ```html
<div class="slider-wrapper">
    <ul class="cat-slider">
	    <li class="slide-ele">
		    <img src="슬라이더 이미지 주소"/>
	    </li>
	    .....
    </ul>
</div>
   ```
위와 같이 3단으로 슬라이더를 구성하고 아래의 css 를 적용합니다.

 ```css
// 액자
.slide-wrapper{
    overflow    : hidden;
}
// 슬라이더 판
.cat-slide{
    padding: 0;
}
// 슬라이드 개별
.cat-slide li{
    float: left;
    list-style: none;
}
 ```
customizing 하더라도 액자의 overflow : hidden
개별 슬라이드(li) 의 float: left 는 반드시 지정해야 합니다.

### 1.2 JS Import
레이아웃 & CSS가 구성되었으면 아래와 같이
jquery , jquery.mobile , catSlider 를 순서대로 넣어줍니다.
```html
<script src="js/jquery-3.5.1.js"></script>
<script src="js/jquery.mobile.custom.js"></script>
<script src="js/jquery.catSlider-1.0.0.js"></script>
```

### 1.3 Slider Start
이미지로드가 완료 된 후 아래의 함수를 호출하면 슬라이더가 준비됩니다.
```javascript
 var setting = {
	wrapperTarget   : $('.slide-wrapper'),	// 액자
     	target          : $('.cat-slide'),	// 슬라이더판
     	childtarget     : $('.cat-slide li'),	// 슬라이더 개별요소

	// 액자하나에 한슬라이드만 볼때 true
	// 슬라이드 하나가 가운데 있고 양옆 반씩 볼때는 false
	// default : false
     	oneImgOnSlide   : false,
     
	// 간격을 줄지 말지 결정 default false
     	space           : true,	
     
     	// space true 일때 간격px 해당 간격의 2배만큼 간격이 생김니다.
     	spacepx         : 10,		
     
     	leftCallBackFunc : function(){
		// 슬라이더가 왼쪽으로 이동 후 실행할 callback 함수
	    	// 사용자가 오른쪽으로 swipe 하면 왼쪽으로 이동합니다.
     	},
     	rightCallBackFunc : function(){
        	// 슬라이더가 오른쪽으로 이동 후 실행할 callback 함수
     	}
 }
 $.catSlider.init(setting);
```
예제와 달리 왼쪽,오른쪽 화살표는 따로 구현되어 있지 않습니다.
화살표를 만들면 아래와 같이 이벤트를 추가하시면 됩니다.
```javascript
$('#prevBtn').on('click',function(){
   // left
   $.catSlider.slideMove(true);
})
$('#nextBtn').on('click',function(){
   // right
   $.catSlider.slideMove(false);
})
```

### 2. 옵션값 추가 설명
#### 2.1 oneImgOnSlide
![enter image description here](https://github.com/icemokacat/cat-slider/blob/main/doc/img/oneslide.png?raw=true)

#### 2.2 setMoveFunc
슬라이더 이동 시 슬라이더판이 움직이는데 이 슬라이더판이 움직이는 정도를
슬라이더 초기화시 $.catSlider.movePx 에 미리 세팅됩니다.
이때 movePx값은 default 로 아래와 같이 세팅됩니다.
```
$.catSlider.movePx = 슬라이더 개별요소 width + (슬라이더 간격이 주어질때) 간격*2
```
슬라이더 구성시 레이아웃 및 CSS Customizing 시 movePx을 임의로 조정해야 할 필요가 있을때 해당 옵션을 사용하셔 movePx을 조정하시면 됩니다.
예)
```javascript
var setting = {
	wrapperTarget	: $('.slide-wrapper'),
	target		: $('.cat-slide'),
	childtarget	: $('.cat-slide li'),
	oneImgOnSlide	: true,
	space		: true,
	spacepx		: 10,
	setMoveFunc : function(){
	   $.catSlider.movePx = 50
	}
}
$.catSlider.init(setting);
```

### 3. 미흡한점
 코드 최적화가 덜 되어 있으며 이미지 재로드시 
 다시 슬라이더를 제대로 세팅할 수 도록 destroy 함수를 구현 중입니다.
