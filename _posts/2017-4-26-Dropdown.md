---
layout: post
title: CSS Dropdown 메뉴 구현하기
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
