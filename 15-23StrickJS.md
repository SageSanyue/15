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
