# SCSS

* parcel-bundler 가 scss 파일을 변환해서
브라우저에서 보여주게 함  

SCSS > CSS 변환 사이트  
https://www.sassmeister.com/

## 중첩 선택
```SCSS
.container { // .container h1 {}
  h1 {
    color:$color;
  }
}
```

## \> -  자식 선택자
```scss
.container { // .container > ul {}
  > ul {
  }
}
```
## & - 상위 선택자 참조
```scss
.btn { // .btn.active{}
  position : absolute;
  &.active{
    color:red;
  }
}
.list { // .list li:last-child{}
  li {
    &:last-child {
      margin-right:0;
    }
  }
}
.fs {
  &-small { font-size:12px;}
  &-medium { font-size:14px;}
  &-large { font-size:16px;}
}
// .fs-small : {font-size:12px;}
```
## 중첩된 속성
```scss
.box {
    font: {
        weight:bold;
        size:10px;
        family:sans-serif;
    };
    margin:{
        top:10px;
        left:20px;
    };
    padding: {
        top:10px;
        bottom:40px;
        left:20px;
        right:30px;
    };
} 
/* .box {
  font-weight: bold;
  font-size: 10px;
  font-family: sans-serif;
  margin-top: 10px;
  margin-left: 20px;
  padding-top: 10px;
  padding-bottom: 40px;
  padding-left: 20px;
  padding-right: 30px;
} */
```

## 변수 (Variables)

```SCSS
$color : royalblue; // 변수 지정
$size : 200px; - 전역 변수
// 재할당 되면 그 아래부터는 바뀐 값이 적용 된다
.container {
  $size: 100px; - container 안 유효범위만 사용가능
    position:fixed;
    top:$size;
    .item {
        width:$size;
        height:$size;
        transform:translteX($size);
    }
}
/* .container {
  position: fixed;
  top: 200px;
}
.container .item {
  width: 200px;
  height: 200px;
  transform: translteX(200px);
} */
```
## @mixin - 재활용 코드
```SCSS
@mixin center {
    display:flex;
    justify-content:center;
    align-items:center;
}
.container {
    @include center;
    .item {
        @include center;
    }
}
.box {
    @include center;
}
/* 
.container {
  display:flex;
  justify-content:center;
  align-items:center;
}
.container .item {
  display:flex;
  justify-content:center;
  align-items:center;
}

.box {
  display:flex;
  justify-content:center;
  align-items:center;
}
*/
@mixin box($size:100px,$color:tomato) {
    width:$size;
    height:$size;
    background-color:$color;
}
.container {
    @include box(200px,red);
    .item {
        @include box($color:green);
    }
}
.box {
    @include box;
}
/* 
.container {
  width: 200px;
  height: 200px;
  background-color: red;
}
.container .item {
  width: 100px;
  height: 100px;
  background-color: green;
}

.box {
  width: 100px;
  height: 100px;
  background-color: tomato;
}
*/
```
## 반복문 @for
```SCSS
@for $i from 1 through 10{
  .box:nth-child(#{$1}){ // scss 보간법 #{변수}
    width:100px * $1;
  }
}
```

## 함수 
```SCSS
@function ratio($size,$ratio){
  @return $size * $ratio
}
.box {
    $width:100px;
    width:$width;
    height:ratio($width, 1/2);
    @include center
}
/* 
.box {
  width: 100px;
  height: 50px;
}
*/
```

## 색상 내장 함수
```SCSS
// mix(color1,color2) 두 색상을 혼합함
.box {
  $color: royalblue;
  width: 200px;
  height: 100px;
  margin: 20px;
  border-radius: 10px;
  background-color:$color;
  &.built-in {
    background-color:mix($color,red);
  }
}

//lighten(color,퍼센트) 퍼센트 만큼 밝게
.box {
  $color: royalblue;
  &.built-in {
    background-color:lighten($color,10%);
  }
}

//darken(color,퍼센트) 퍼센트 만큼 어둡게
  &.built-in {
    background-color:darken($color,10%);
  }

//saturate(color,10%) 채도를 퍼센트만큼 올림
  &.built-in {
    background-color:saturate($color,10%);
  }

//desaturate(color,10%) 채도를 퍼센트만큼 낮춤
  &.built-in {
    background-color:desaturate($color,10%);
  }

//grayscale(color) 그레이로 만듬
&.built-in {
    background-color:grayscale($color);
  } 

//invert($color) 색상 반전
&.built-in {
    background-color:invert($color);
  } 
  
//rgba($color,0.5) 투명도
&.built-in {
    background-color:rgba($color,.5);
  } 
```
## SCSS 데이터 종류
```SCSS
$number : 1; //.5,100px,1em
$string:bold; //relative,"../images/a.png"
$color:red; // blue,#fff00,rgba(0,0,0,0.1);
$boolean:true; //false
$null:null;
$list: orange,royalblue,yellow;
$map: (
  o:orange,
  r:royalblue,
  y:yellow
)

@each $c in $list { //list에 값들이 $c에 저장되어 반복 출력됨
  .box {
    color : $c;
  }
}

@each $key, $value in $map { 
  .box-#{$key} {
    color :$value;
  }
}
```

## @content 
```scss
@mixin left-top { //새로 작성한 내용이 @content 부분으로 들어감
  position:absolute;
  top:0;
  left:0;
  @content;
}
.box {
  width:200px;
  height:300px;
  @include left-top {
    bottom:0;
    right:0;
    margin:auto;
  }
}
```