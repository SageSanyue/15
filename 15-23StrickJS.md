## Loading Animation
1. 用两个不断变化的圆圈表示
```
<body>
<div class="wrapper">
  <div class="circle"></div>
  <div class="circle"></div>
</div>
</body>
.wrapper{
  width: 200px;
  height: 200px;
  border: 1px solid red;
  position: relative;
}
.circle{
  position: absolute;
  width: 10px;
  height: 10px;
  background: black;
  border-radius: 50%;
  top: 0;
  left: 0;
  bottom: 0;
  right: 0;
  margin: auto;
  animation: s 1s linear infinite;
}
.circle:nth-child(2){
  animation-delay: 0.75s;
}
@keyframes s{
  0%{
    width: 0;height: 0;opacity: 1;
  }
  100%{
    width: 100px;height: 100px;opacity: 0;
  }
}
```
效果预览http://js.jirengu.com/tosim/2/edit

其实.circle可去掉，仅在CSS上加before、after即可达到效果：
```
<body>
<div class="wrapper">
  
</div>
</body>

.wrapper{
  width: 200px;
  height: 200px;
  border: 1px solid red;
  position: relative;
}
.wrapper::before,.wrapper::after{
  content:'';
  position: absolute;
  width: 0px;
  height: 0px;
  background: black;
  border-radius: 50%;
  top: 0;
  left: 0;
  bottom: 0;
  right: 0;
  margin: auto;
  animation: s 1s linear infinite;
}
.wrapper::after{
  animation-delay: 0.75s;
}
@keyframes s{
  0%{
    width: 0;height: 0;opacity: 1;
  }
  100%{
    width: 100px;height: 100px;opacity: 0;
  }
}
```

http://js.jirengu.com/qamav/1/edit?html,css,output
改名叫“loading”便可把这个CSS给其他复用
https://jsbin.com/pusugux/edit?html,css,js,output

## Sticky NavBar and Underline
1.sticky navbar
```
.topNavBar {
    padding: 20px 0;
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    transition: all 1s;  //添加动画
    color: #b7b7b7;
}

.topNavBar.sticky{
    background: white;
    padding: 10px 0;
    z-index: 1;
    box-shadow: 0 0 10px rgba(0,0,0,0.25);  与下边区分开
    color: inherit;
}
```
2.Underline Animation
```
.topNavBar  nav > ul > li > a {
    color: inherit;
    font-size: 12px;
    font-weight: bold;
    border-top: 3px solid transparent;
    border-bottom: 3px solid transparent;
    padding-top: 5px;
    padding-bottom: 5px;
    display: block;
    text-decoration: none;
    position: relative;    //此句新增
}
.topNavBar nav > ul > li > a:hover::after{
    content:'';
    display: block;
    position: absolute;
    top: 100%;
    left: 0;
    background: #f94a5e;
    width: 100%;
    height: 3px;
    animation: menuSlide .3s;
}
@keyframes menuSlide{
    /*0%{
        transform: translateX(-100%);    // 效果错误，应为width
    }
    100%{
        transform: translateX(0);
    }*/
    0%{
        width: 0;
    }
    100%{
        width: 100%;
    }
}
/*.topNavBar  nav > ul > li > a:hover {
    border-bottom: 3px solid #e06567;
} */
```
