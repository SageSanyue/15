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
## 导航栏二级菜单
1.li的a标签下加二级菜单
加在a内外皆可，惯例加在a外边。
```
let aTags = document.getElementsByClassName('menuTrigger')
             //console.log(aTags)
             for(let i=0;i<aTags.length;i++){
                 aTags[i].onmouseenter = function(x){
                     //console.log('mouseenter')
                     /*console.log(x.target)
                     console.log(x.currentTarget)  //假如a标签内还有span标签，onclick事件监听上句打印span此句打印a
                     */
                     let a = x.currentTarget   //建议使用curenttarget不会出错 
                     //console.log(a.nextSibling.nextSibling)
                     let brother = a.nextSibling
                     while(brother.nodeType === 3){    //找a的节点弟弟而非文本弟弟,或者改写成while(brother.tagName!=='UL')
                        brother = brother.nextSibling
                     }
                     brother.classList.add('active')
                 }
                 aTags[i].onmouseleave = function(x){
                    let a = x.currentTarget
                    let brother = a.nextSibling
                    while(brother.tagName !== 'UL'){
                       brother = brother.nextSibling
                    }
                    brother.classList.remove('active')
                 }
           }
```
```
.topNavBar .submenu{
    display: none;
    position: absolute;
    left: 0;
    top: 100%;

}
.topNavBar .submenu.active{
    display: block;
}
.topNavBar .submenu > li{
    white-space: nowrap;   /*二级菜单每一项不换行*/
}

               <li>
                   <a class="menuTrigger" href="#">作品</a>
                    <ul class="submenu">
                        <li>作品1</li>
                        <li>作品2</li>
                        <li>作品3</li>
                    </ul>
                </li>
               <li>
                   <a class="menuTrigger" href="#">博客</a>
                   <ul class="submenu">
                        <li>博客1</li>
                        <li>博客2</li>
                        <li>博客3</li>
                    </ul>
                </li>
```
改进鼠标移开无法进一步点选二级菜单的bug:
1.监听对象应该是a与ul的共同标签li
```
                <li class="menuTrigger">
                   <a  href="#">作品</a>
                    <ul class="submenu">
                        <li>作品1</li>
                        <li>作品2</li>
                        <li>作品3</li>
                    </ul>
                </li>
               <li class="menuTrigger">
                   <a href="#">博客</a>
                   <ul class="submenu">
                        <li>博客1</li>
                        <li>博客2</li>
                        <li>博客3</li>
                    </ul>
                </li>
```
```
//let aTags = document.getElementsByClassName('menuTrigger') 为改进BUG改进鼠标移开无法进一步点选二级菜单，监听对象应该是a与ul的共同标签li
             let liTags = document.getElementsByClassName('menuTrigger')
             //console.log(aTags)
             //for(let i=0;i<aTags.length;i++){
            for(let i=0;i<liTags.length;i++){
                 //aTags[i].onmouseenter = function(x){
                liTags[i].onmouseenter = function(x){
                     //console.log('mouseenter')
                     /*console.log(x.target)
                     console.log(x.currentTarget)  //假如a标签内还有span标签，onclick事件监听上句打印span此句打印a
                     */
                     //let a = x.currentTarget   //建议使用curenttarget不会出错 
                    let li = x.currentTarget
                     //console.log(a.nextSibling.nextSibling)
                     //let brother = a.nextSibling
                     let brother = li.getElementsByTagName('ul')[0]
                     /*while(brother.nodeType === 3){    //找a的节点弟弟而非文本弟弟,或者改写成while(brother.tagName!=='UL')
                        brother = brother.nextSibling
                     }*/
                     brother.classList.add('active')
                 }
                 //aTags[i].onmouseleave = function(x){
                liTags[i].onmouseleave = function(x){
                    let li = x.currentTarget
                    let brother = li.getElementsByTagName('ul')[0]
                    //while(brother.tagName !== 'UL'){
                    //   brother = brother.nextSibling
                    //}
                    brother.classList.remove('active')
                 }
             }
```

进一步修改
```
/*.topNavBar .submenu.active{
    display: block;
}*/
.topNavBar li.active > .submenu{
    display: block;
}

 let liTags = document.getElementsByClassName('menutrigger')
             for(){
                 liTags[i].onmouseenter = function(x){
                    x.currentTarget.classList.add('active')
                 }
                 liTags[i].onmouseleave = function(){
                     x.currentTarget.classList.remove('active')
                 }
}
```

```
let liTags = document.querySelectorAll('nav.menu > ul >li')
             for(let i=0;i<liTags.length;i++){
                 liTags[i].onmouseenter = function(x){
                    x.currentTarget.classList.add('active')
                 }
                 liTags[i].onmouseleave = function(x){
                     x.currentTarget.classList.remove('active')
                 }
}

          <nav class="menu" style="float: right;">
           <ul class="clearfix">
               <li><a href="#">关于</a></li>
               <li><a href="#">技能</a></li>
               <li class="menuTrigger">
                   <a  href="#">作品</a>
                    <ul class="submenu">
                        <li>作品1</li>
                        <li>作品2</li>
                        <li>作品3</li>
                    </ul>
                </li>
               <li class="menuTrigger">
                   <a href="#">博客</a>
                   <ul class="submenu">
                        <li>博客1</li>
                        <li>博客2</li>
                        <li>博客3</li>
                    </ul>
                </li>
               <li><a href="#">日历</a></li>
               <li><a href="#">联系方式</a></li>
               <li><a href="#">其他</a></li>
           </ul>
           </nav>  
```
