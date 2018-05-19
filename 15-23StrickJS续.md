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
1.去https://cdnjs.com  输入tween,在Library里找到tween.js,复制 Link。 https://github.com/tweenjs/tween.js/blob/master/docs/user_guide.md

http://js.jirengu.com/wilar/1/edit?html,js,output
```
function animate(time){
  requestAnimationFrame(animate);
  TWEEN.update(time);
}
requestAnimationFrame(animate);
var coords = {x:0};
var tween = new TWEEN.Tween(coords)
         .to({x:1000},1000)
         .easing(TWEEN.Easing.Quadratic.In)
         .onUpdate(function(){
           console.log(coords.x);
         })
         .start();
```
代码片段插入到for(let i=0;i<aTags.length;i++){前，
```
           let aTags = document.querySelectorAll('nav.menu > ul > li > a')
             //console.log(aTags)

             function animate(time){
                requestAnimationFrame(animate);
                TWEEN.update(time);
            }
            requestAnimationFrame(animate);
    
             for(let i=0;i<aTags.length;i++){
                aTags[i].onclick = function(x){
                   x.preventDefault()  //阻止默认事件
                   let a = x.currentTarget
                   let href = a.getAttribute('href')   //'#siteAbout'
                   let element = document.querySelector(href)
                   let top = element.offsetTop

                    let currentTop = window.scrollY
                    let targetTop = top-80
                    var coords = {y:currentTop};
                    var tween = new TWEEN.Tween(coords)
                    .to({y:targetTop},1000)
                    .easing(TWEEN.Easing.Quadratic.In)
                    .onUpdate(function(){
                         window.scrollTo(0,coords.y)
                    })
                    .start();
                  }
                 }
```
随后尝试改.easing(TWEEN.Easing.Quadratic.In)为.easing(TWEEN.Easing.Quadratic.InOut)//淡入淡出
```
             let aTags = document.querySelectorAll('nav.menu > ul > li > a')

             function animate(time){
                requestAnimationFrame(animate);
                TWEEN.update(time);
            }
            requestAnimationFrame(animate);      //请求动画帧率
    
             for(let i=0;i<aTags.length;i++){
                aTags[i].onclick = function(x){
                   x.preventDefault()  //阻止默认事件
                   let a = x.currentTarget
                   let href = a.getAttribute('href')   //'#siteAbout'
                   let element = document.querySelector(href)
                   let top = element.offsetTop

                    let currentTop = window.scrollY
                    let targetTop = top-80
                    let s = targetTop - currentTop
                    var coords = {y:currentTop}
                    var t = Math.abs((s/100)*300)   //距离s可以为负，但时间不能为负
                    //var t = (s/100)*300
                    if(t>500){t = 500}
                    var tween = new TWEEN.Tween(coords)
                    .to({y:targetTop},t)
                    .easing(TWEEN.Easing.Quadratic.InOut)   //淡入淡出
                    .onUpdate(function(){
                         window.scrollTo(0,coords.y)
                    })
                    .start();
                   }
                }
```
## 除点菜单页面动外增加相反功能
1.第一步，先打出y坐标观察
```
             window.onscroll = function(x){
                if(window.scrollY > 0){
                    topNavBar.classList.add('sticky')
                }else{
                    topNavBar.classList.remove('sticky')
                }
                console.log(window.scrollY)
             }
```
然后对页面中元素进行标记
```
<main>
            <div data-x id="siteAbout" class="userCard">
            ...
</main>
            <section data-x id="siteSkills" class="skills">
            ...
             </section>
            <section data-x id="siteWorks" class="portfolio">
            ...
```
获取三个标记的元素的offsetTop
```
                console.log(window.scrollY)
                let specialTags = document.querySelectorAll('[data-x]')  //含有data-x属性的标签
                for(let i=0;i<specialTags.length;i++){
                    console.log(specialTags[i].offsetTop)
                }
```
```
              window.onscroll = function(x){
                if(window.scrollY > 0){
                    topNavBar.classList.add('sticky')
                }else{
                    topNavBar.classList.remove('sticky')
                }
                console.log('window.scrollY')
                console.log(window.scrollY)
                let specialTags = document.querySelectorAll('[data-x]')  //含有data-x属性的任意标签
                for(let i=0;i<specialTags.length;i++){
                    console.log('specialTags[i].offsetTop')
                    console.log(specialTags[i].offsetTop)
                }
             }
```
可以在控制台观察到仅window.scrollY随滚动而变化，specialTags[i].offsetTop无变化
2.思路：找出距离当前滚动距离最近的一个标签
```
             window.onscroll = function(x){
                if(window.scrollY > 0){
                    topNavBar.classList.add('sticky')
                }else{
                    topNavBar.classList.remove('sticky')
                }
                console.log('window.scrollY')
                console.log(window.scrollY)
                let specialTags = document.querySelectorAll('[data-x]')  //含有data-x属性的任意标签
                let minIndex = 0
                for(let i=0;i<specialTags.length;i++){
                    if(Math.abs(specialTags[i].offsetTop - window.scrollY) < Math.abs(specialTags[minIndex].offsetTop - window.scrollY)){
                        minIndex = i
                    }
                    console.log(minIndex)
                }
             }
```
```
           window.onscroll = function(x){
                if(window.scrollY > 0){
                    topNavBar.classList.add('sticky')
                }else{
                    topNavBar.classList.remove('sticky')
                }
                console.log('window.scrollY')
                console.log(window.scrollY)
                let specialTags = document.querySelectorAll('[data-x]')  //含有data-x属性的任意标签
                let minIndex = 0
                for(let i=0;i<specialTags.length;i++){
                    if(Math.abs(specialTags[i].offsetTop - window.scrollY) < Math.abs(specialTags[minIndex].offsetTop - window.scrollY)){
                        minIndex = i
                    }
                    //console.log(minIndex)
                }
                specialTags[minIndex].classList.add('active')
             }
```
我们把它找到后标识出来
```
/*h1,h2,h3,h4,h5,h6 {
    font-weight: normal;
} 新加CSS语句的地方之前一句*/

[data-x].active{
    outline: 10px solid red;
}
```
```
           window.onscroll = function(x){
                if(window.scrollY > 0){
                    topNavBar.classList.add('sticky')
                }else{
                    topNavBar.classList.remove('sticky')
                }
                console.log('window.scrollY')
                console.log(window.scrollY)
                let specialTags = document.querySelectorAll('[data-x]')  //含有data-x属性的任意标签
                let minIndex = 0
                for(let i=0;i<specialTags.length;i++){
                    if(Math.abs(specialTags[i].offsetTop - window.scrollY) < Math.abs(specialTags[minIndex].offsetTop - window.scrollY)){
                        minIndex = i
                    }
                    //console.log(minIndex)
                }
                for(let i=0;i<specialTags.length;i++){
                    specialTags[i].classList.remove('active')
                }
                specialTags[minIndex].classList.add('active')
             }
```
3.找对应的菜单栏标签
```
                specialTags[minIndex].classList.add('active')
                let id = specialTags[minIndex].id
                console.log(id)
```
```
                specialTags[minIndex].classList.add('active')
                let id = specialTags[minIndex].id
                //console.log(id)
                let a = document.querySelector('a[href="#' + id + '"]')  //例如id=='siteAbout',则对应的a标签'a[href="#siteAbout"]'
                let li = a.parentNode
                console.log(li)

             }

```

4.找到后开始绑定对应事件
```
                specialTags[minIndex].classList.add('active')
                let id = specialTags[minIndex].id
                //console.log(id)
                let a = document.querySelector('a[href="#' + id + '"]')  //例如id=='siteAbout',则对应的a标签'a[href="#siteAbout"]'
                let li = a.parentNode
                //console.log(li)
                let brothersAndMe = li.parentNode.children
                for(let i=0;i<brothersAndMe.length;i++){
                    brothersAndMe[i].classList.remove('active')
                }
                li.classList.add('active')
             }

```
滑动到相应章节页面顶部sticky的相应NavBar选项被点亮，功能实现；但是出现了二级菜单随之自动冒出的BUG 
5.解决BUG
鼠标滚动带来的active使NavBar对应项高亮与鼠标在其上移入移除对应高亮的两个效果冲突，解决办法：换名
```
            window.onscroll = function(x){
                if(window.scrollY > 0){
                    topNavBar.classList.add('sticky')
                }else{
                    topNavBar.classList.remove('sticky')
                }
                let specialTags = document.querySelectorAll('[data-x]')  //含有data-x属性的任意标签
                let minIndex = 0
                for(let i=0;i<specialTags.length;i++){
                    if(Math.abs(specialTags[i].offsetTop - window.scrollY) < Math.abs(specialTags[minIndex].offsetTop - window.scrollY)){
                        minIndex = i
                    }
                  }
              let id = specialTags[minIndex].id
              let a = document.querySelector('a[href="#' + id + '"]')  //例如id=='siteAbout',则对应的a标签'a[href="#siteAbout"]'
              let li = a.parentNode
              let brothersAndMe = li.parentNode.children
                for(let i=0;i<brothersAndMe.length;i++){
                    //brothersAndMe[i].classList.remove('active')
                    brothersAndMe[i].classList.remove('highlight')
                }
                //li.classList.add('active')
                li.classList.add('highlight')
             }
```
对应CSS 变化
```
/*.topNavBar nav > ul > li.active > a::after{  */
.topNavBar nav > ul > li.active > a::after,
.topNavBar nav > ul > li.highlight > a::after{
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
```
## 让所有章节都从下往上显现
1.
```
                let specialTags = document.querySelectorAll('[data-x]')  //含有data-x属性的任意标签
                let minIndex = 0
                for(let i=0;i<specialTags.length;i++){
                    if(Math.abs(specialTags[i].offsetTop - window.scrollY) < Math.abs(specialTags[minIndex].offsetTop - window.scrollY)){
                        minIndex = i
                    }
                 }//minIndex就是离窗口顶部最近的元素
                 specialTags[minIndex].classList.add('animate')
                 
```
效果不太对， 
```
             setTimeout(function(){
                siteWelcome.classList.remove('active')
             },1000)

             //添加offset类 
              let specialTags = document.querySelectorAll('[data-x]')  //含有data-x属性的任意标签
              for(let i=0;i<specialTags.length;i++){
                    specialTags[i].classList.add('offset')
              }

             window.onscroll = function(x){
                if(window.scrollY > 0){
                    topNavBar.classList.add('sticky')
                }else{
                    topNavBar.classList.remove('sticky')
                }
                let specialTags = document.querySelectorAll('[data-x]')  //含有data-x属性的任意标签
                let minIndex = 0
                for(let i=0;i<specialTags.length;i++){
                    if(Math.abs(specialTags[i].offsetTop - window.scrollY) < Math.abs(specialTags[minIndex].offsetTop - window.scrollY)){
                        minIndex = i
                    }
                 }//minIndex就是离窗口顶部最近的元素
                 //specialTags[minIndex].classList.add('animate')
                 specialTags[minIndex].classList.remove('offset')
                 
```
```
[data-x].offset{
    transform: translateY(100px);
}
[data-x]{
    transform: translateY(0);
    transition: all 0.5s;
}
.topNavBar {
    padding: 20px 0;
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    transition: all 1s;
    color: #b7b7b7;
}

```
但首章节“ 关于”没有反映，所以需要让li.classList.add('highlight')的操作在scroll滚动发生前就执行一次，将该部分整合成函数，先调用一次，再在window.onscroll = function(x){中复用一下。
```
             let specialTags = document.querySelectorAll('[data-x]')  //含有data-x属性的任意标签
              for(let i=0;i<specialTags.length;i++){
                    specialTags[i].classList.add('offset')
              }

              slide()

              window.onscroll = function(x){
                if(window.scrollY > 0){
                    topNavBar.classList.add('sticky')
                }else{
                    topNavBar.classList.remove('sticky')
                }
                slide()
              }
              
              function slide(){
                let specialTags = document.querySelectorAll('[data-x]')  //含有data-x属性的任意标签
                let minIndex = 0
                for(let i=0;i<specialTags.length;i++){
                    if(Math.abs(specialTags[i].offsetTop - window.scrollY) < Math.abs(specialTags[minIndex].offsetTop - window.scrollY)){
                        minIndex = i
                    }
                }
                specialTags[minIndex].classList.remove('offset')
                let id = specialTags[minIndex].id
                let a = document.querySelector('a[href="#' + id + '"]')  //例如id=='siteAbout',则对应的a标签'a[href="#siteAbout"]'
                let li = a.parentNode
                let brothersAndMe = li.parentNode.children
                for(let i=0;i<brothersAndMe.length;i++){
                    brothersAndMe[i].classList.remove('highlight')
                }
                li.classList.add('highlight')
             }

```
优化
```
              //添加offset类 
              let specialTags = document.querySelectorAll('[data-x]')  //含有data-x属性的任意标签
              for(let i=0;i<specialTags.length;i++){
                    specialTags[i].classList.add('offset')
              }
              
              setTimeout(function(){
                slide()
              },1000)

              window.onscroll = function(x){
                if(window.scrollY > 0){
                    topNavBar.classList.add('sticky')
                }else{
                    topNavBar.classList.remove('sticky')
                }
                slide()
              }
              
```
## 技能部分进度条动态显示
