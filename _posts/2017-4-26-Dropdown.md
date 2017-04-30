---
layout: post
title: 웹알못 CSS Dropdown 메뉴 구현하기
category: programming
---

![](/images/2017-4-26-Dropdown/css.png)

블로그를 운영하면서 웹을 조금씩 건들이고 있다.
CSS로 Dropdown 메뉴를 구현하기 위해 w3schools.com
[CSS Dropdown](https://www.w3schools.com/css/css_dropdowns.asp)
문서를 참고해 구현 하였다. ~~이쁘지 않아서 현재는 제거했다.~~

[ 예제 코드 ]  

```html
<style>
.dropdown {
    padding: 5px 5px 5px 5px;
    position: relative;
    display: inline-block;
    background-color: #494949;
    color: #f9f9f9;
    border-radius: 3px;
}

.dropdown-content {
    display: none;
    position: absolute;
    background-color: #494949;
    min-width: 160px;
    box-shadow: 0px 8px 16px 0px rgba(0,0,0,0.2);
    padding: 12px 16px;
    z-index: 1;
}

.dropdown:hover .dropdown-content {
    display: block;
}
</style>

<div class="dropdown">
  <span>Mouse over me</span>
  <div class="dropdown-content">
    <p>Hello World!</p>
  </div>
</div>
```

[ 결과물 ]  

<style>
.dropdown {
  padding: 5px 5px 5px 5px;
  position: relative;
  display: inline-block;
  background-color: #494949;
  color: #f9f9f9;
  border-radius: 3px;
}

.dropdown-content {
    display: none;
    position: absolute;
    background-color: #494949;
    min-width: 160px;
    box-shadow: 0px 8px 16px 0px rgba(0,0,0,0.2);
    padding: 12px 16px;
    z-index: 1;
}

.dropdown:hover .dropdown-content {
    display: block;
}
</style>

<div class="dropdown">
  <span>Mouse over me</span>
  <div class="dropdown-content">
    <p>Hello World!</p>
  </div>
</div>


핵심은 `hover` 상태가 아니면 서브매뉴 여기선 `dropdown-content` 의 `display` 값을   
`none`으로 하여 숨기고 있다, `hover` 상태가 되면 `display: block` 으로 보여주는 것이다.

이렇게 하면 마우스가 올라가 있으면 `dropdown-content` 클래스의 항목을 출력하게 되고,
마우스를 내리면 숨기는 식으로 진행된다.

다음은 내가 실습한 결과물이다.
<style>
#blogMenu {
	float:left;
	margin:2px 2px 5px 5px;
	padding:5px 5px 10px 10px;
	box-shadow: 0px 0px 15px rgba(0,0,0,.3);
	-moz-box-shadow: 0px 0px 15px rgba(0,0,0,.3);
	-webkit-box-shadow: 0px 0px 15px rgba(0,0,0,.3);
	-o-box-shadow: 0px 0px 15px rgba(0,0,0,.3);
	-moz-border-radius: 3px;
	-khtml-border-radius: 3px;
	-webkit-border-radius: 3px;
	border-radius: 5px;
	background-color:#000000;
}

#blogMenu ul li {
    margin:0px;
	padding:0px;
	float:left;
	list-style-type:none;
}

#blogMenu ul li:hover ul {
	display: block;
}

#blogMenu ul ul {
	margin:3px 0px 0px 0px;
	padding:0px 0px 5px 10px;
	display:none;
	position:absolute;
	background-color:#000000;
	border-radius: 5px;
	border-top-left-radius: 0px;
}

#blogMenu ul ul li {
	float:none;
}

#blogMenu a {
	height:16px;
	color:#f1f1f1;
	font-family:arial;
	font-size:12px;
	padding:0px 10px 0px 0px;
	text-decoration:none;
}

#blogMenu a:hover {
	color:#D4F4FA;
	border-bottom:3px solid #FAED7D;
}
</style>


  <h1>FuZer's HTML & CSS test</h1>

  <div id="blogMenu">
    <ul>
      <li><a href="#">Menu 1</a></li>
      <li><a href="#">Menu 2</a>
        <ul>
          <li><a href="#">Sub Menu 1</a></li>
          <li><a href="#">Sub Menu 2</a></li>
          <li><a href="#">Sub Menu 3</a></li>
        </ul>
      </li>
      <li><a href="#">Menu 3</a>
        <ul>
          <li><a href="#">Sub Menu 1</a></li>
          <li><a href="#">Sub Menu 2</a></li>
          <li><a href="#">Sub Menu 3</a></li>
        </ul>
      </li>
      <li><a href="#">Menu 4</a></li>
      <li><a href="#">Menu 5</a></li>
    </ul>
  </div>

<br><br><br>
