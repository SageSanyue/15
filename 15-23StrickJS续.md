## 点击导航栏相应部分跳转
1.先尝试给<a href="#"></a>的#添加对应ID
```
<li><a href="#siteAbout">关于</a></li>

<li><a href="#siteSkills">技能</a></li>

<a  href="#siteWorks">作品</a>

...
<main>
            <div id="siteAbout" class="userCard">
            ...
 </main>
        <section id="siteSkills" class="skills"> 
        ...
        </section>
          <section id="siteWorks" class="portfolio">
          ...
```
但是发现顶部NavBar遮盖了相应部分的标题，为了不改CSS的padding-top样式，选择尝试JS操作：
```
 let aTags = document.querySelectorAll('nav.menu > ul > li > a')
             //console.log(aTags)
             for(let i=0;i<aTags.length;i++){
                aTags[i].onclick = function(x){
                   x.preventDefault()  //阻止默认事件
                   //console.log('you clicked');console.log(x.currentTarget)
                   let a = x.currentTarget
                   //console.log(a.href)   被浏览器处理过的href
                   //console.log(a.getAttribute('href'))  //代码中coder定义的href
                   let href = a.getAttribute('href')   //'#siteAbout'
                   let element = document.querySelector(href)
                   debugger
                   console.log(element)
                }
             }             
```
加了断点debugger在控制台操作，
```
element.getBoundingClientRect()
```
可以得到一个距离各方位(返回元素大小及其相对于视口的位置 )的hash:DOMRect {x: 162.5, y: 978, width: 940, height: 314, top: 978, …}
于是尝试使用element.offsetTop
```
             for(let i=0;i<aTags.length;i++){
                aTags[i].onclick = function(x){
                   x.preventDefault()  //阻止默认事件
                   //console.log('you clicked');console.log(x.currentTarget)
                   let a = x.currentTarget
                   //console.log(a.href)   被浏览器处理过的href
                   //console.log(a.getAttribute('href'))  //代码中coder定义的href
                   let href = a.getAttribute('href')   //'#siteAbout'
                   let element = document.querySelector(href)
                   //debugger
                   //let rect = element.getBoundingClientRect()
                   //console.log(rect.top)
                   console.log(element.offsetTop) //元素距页面最顶端的距离，而不是距窗口顶端的距离
                }
             }  
```
```
let aTags = document.querySelectorAll('nav.menu > ul > li > a')
for(let i=0;i<aTags.length;i++){
   aTags[i].onclick = function(x){
     x.preventDefault()  //阻止默认事件
     let a = x.currentTarget
     let href = a.getAttribute('href')   //'#siteAbout'
     let element = document.querySelector(href)
     let top = element.offsetTop
     window.scrollTo(0,top-80)     //滑动到X方向不变Y方向减去80
}
```
## 点击导航栏相应跳转较生硬的优化
与滚动条有关的动画无法用CSS实现，只能用就是js实现。
一个简易的JS动画教程：http://js.jirengu.com/kinuv/1/edit?html,js,output
```
<body>
  <div id="div1"></div>
</body>

div1.style.position = 'relative'
div1.style.left = 0

var n=0
var id = setInterval(()=>{
  n = n + 1
  div1.style.left = n + 'px'
  if(n>=200){
    window.clearInterval(id)
  }
},40)
```
```
                   let n = 25 //一共动的次数
                   let duration = 500 / n      //动一次多长时间
                   let currentTop = window.scrollY
                   let targetTop = top-80
                   let distance = (targetTop - currentTop)/n
                   let i = 0
                   /*
                   console.log(targetTop)
                   console.log(currentTop)  //console.log调试大法
                   console.log(n)
                   console.log(distance)
                   */
                   var id = setInterval(()=>{
                    if(i === n){
                        window.clearInterval(id)
                        return
                    }
                    i=i+1
                    window.scrollTo(0,currentTop + distance * i)
                    //window.scrollTo(0,targetTop + distance*i)
                   },duration)
```
## 优化之缓动函数

