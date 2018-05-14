# canvas教程
## 一、原生写法
原生js的写法：
```
var div = document.getElementById('canvas')
var painting = false

//按下鼠标
div.onmousedown = function(a){
  painting = true
  var x = a.clientX
  var y = a.clientY
  var divA = document.createElement('div')
  divA.style = "width:6px;height:6px;"+
    "background:black;border-radius:3px;"+
    "position:absolute;left:"+(x-3)+"px;"+
    "top:"+(y-3)+"px;"
  div.appendChild(divA)
}
div.onmousemove = function(a){
  if(painting){
    painting = true
  var x = a.clientX
  var y = a.clientY
  var divA = document.createElement('div')
  divA.style = "width:6px;height:6px;"+
    "background:black;border-radius:3px;"+
    "position:absolute;left:"+(x-3)+"px;"+
    "top:"+(y-3)+"px;"
  div.appendChild(divA)
  }else{
    
  }
}
div.onmouseup = function(z){
  painting = false
}
```
http://js.jirengu.com/nuvan/1/edit
## 二、Canvas画简单图形
画矩形、三角形、圆形：
```
var yyy = document.getElementById('xxx');
var context = yyy.getContext('2d')

//描边
context.fillStyle = 'red'
context.fillRect(10,0,100,100)
//填充
context.strokeStyle = 'blue'
context.strokeRect(10,0,100,100)
//橡皮擦
context.clearRect(50,50,10,10)

//画矩形
context.fillStyle = 'green'
context.beginPath()
context.moveTo(240,240)
context.lineTo(300,240)
context.lineTo(300,300)
context.fill()
 
//画圆
context.beginPath()
context.arc(150,150,30,0, Math.PI*2) //半径30,0度～360度 （弧度制）
context.fill()

context.beginPath()
context.arc(50,250,50,0,2)
context.fill()

context.beginPath()
context.arc(10,200,50,0,2)
context.stroke()   //只描边
```
http://js.jirengu.com/kopen/1/edit?html,css,js,output
## 三、canvas 画直线
```
var context = xxx.getContext('2d')
context.beginPath()
context.moveTo(0,0)
context.lineWidth = 5
context.lineTo(200,0)
context.stroke()     //6步顺序不可乱
context.closePath()
```
http://js.jirengu.com/suvip/1/edit

## 四、简易画板（可连续画线）
http://js.jirengu.com/gebeb/1/edit?html,js,output

## 五、优化功能：全屏
```
var pageWidth = document.documentElement.clientWidth
var pageHeight = document.documentElement.clientHeight //背下来这两句求页面宽高(IE不支持)

yyy.width = pageWidth
yyy.height = pageHeight

//防止用户改变窗口宽高，故监听
window.onresize = function(){
  var pageWidth = document.documentElement.clientWidth
  var pageHeight = document.documentElement.clientHeight //背下来这两句求页面宽高(IE不支持)

  yyy.width = pageWidth
  yyy.height = pageHeight
}
```
 代码预览：http://js.jirengu.com/vikak/1/edit?html,js,output
## 六、优化功能：增加橡皮擦功能
```
var yyy = document.getElementById('xxx')

userResize()
window.onresize = function(){
  userResize()
}
function userResize(){
  var pageWidth = document.documentElement.clientWidth
  var pageHeight = document.documentElement.clientHeight //背下来这两句求页面宽高(IE不支持)

  yyy.width = pageWidth
  yyy.height = pageHeight
}

var context = yyy.getContext('2d')
/*var painting = false //改名叫using*/
var using = false
var lastPoint = {x:undefined,y:undefined}
yyy.onmousedown = function(aaa){
var x = aaa.clientX 
  var y = aaa.clientY
  if(eraserEnabled){
    using = true
    context.clearRect(x-5,y-5,10,10)//矩形是把做左上角当起点画，故要减去一点才是矩形中间
  }else{
    using = true
    lastPoint = {"x":x,"y":y}
  }
}
yyy.onmousemove = function(aaa){
var x = aaa.clientX
  var y = aaa.clientY
  if(eraserEnabled){
    if(using){
    context.clearRect(x,y,10,10)
    }
  }else{
    if(using){
      var newPoint = {"x":x,"y":y}
      drawLine(lastPoint.x,lastPoint.y,newPoint.x,newPoint.y)
      lastPoint = newPoint
    }
  }

}
yyy.onmouseup = function(aaa){
  using = false
}
function drawLine(x1,y1,x2,y2){
  context.beginPath()
  context.strokeStyle = 'black'
  context.moveTo(x1,y1) //起点 
  context.lineWidth = 3
  context.lineTo(x2,y2)  //终点
  context.stroke()
  context.closePath()
}
var eraserEnabled = false
 eraser.onclick = function(){
   eraserEnabled = !eraserEnabled
 }
 ```
 代码预览:http://js.jirengu.com/boveq/1/edit?html,js,output
 
  优化结构：
  http://js.jirengu.com/rihux/1/edit?html,js,output
##  七、再次优化函数结构+橡皮擦使用状态显示
```
 var eraserEnabled = false
 eraser.onclick = function(){
   eraserEnabled = !eraserEnabled
   if(eraserEnabled){
     eraser.textContent = '画笔'
   }else{
     eraser.textContent = '橡皮擦'
   }
 }
```
代码预览:http://js.jirengu.com/capen/1/edit?html,js,output
但以上一个按钮做了两件事，应该改为一个按钮只有一个功能,这样就走正常流程而不用有if-else结构。如下：
```
<div id=actions class="actions">
<button id=eraser>橡皮擦</button>
<button id=brush>画笔</button>
</div>

button{
  position: fixed;
  bottom: 0;
  left: 0;
}
.actions > #brush{
  display: none;
}
.actions.x > #brush{
  display: inline-block;
}
.actions.x > #eraser{
  display: none;
}

eraser.onclick = function(){
   eraserEnabled = true
   actions.className = 'actions x'
 }
 brush.onclick = function(){
   eraserEnabled = false
   actions.className = 'actions'
 }
```
代码预览：http://js.jirengu.com/cupil/1/edit?html,css,js,output
##  八、适应移动端
.onmousemove事件在手机屏幕失效，故需改用.ontouchmove事件  
手指事件：canvas.ontouchstart = funcion(){}  canvas.ontouchmove = function(){} canvas.ontouchend = function(){}
```
function listenToUser(canvas){
    var using = false
    var lastPoint = {x:undefined,y:undefined}

    //特性检测
    if(document.body.ontouchstart !== undefined){
      //触屏设备
      canvas.ontouchstart = function(aaa){
        //console.log(aaa)  //可以看到TouchEvent的hash里有touches的数组，[0]表示屏幕上第一个触摸的点
        var x = aaa.touches[0].clientX
        var y = aaa.touches[0].clientY
        console.log(x.y)
        using = true
        if(eraserEnabled){
          context.clearRect(x-5,y-5,10,10)
        }else{
          lastPoint = {"x":x,"y":y}
        }
      }
      canvas.ontouchmove = function(aaa){
        //console.log('边摸边动')
        var x = aaa.touches[0].clientX
        var y = aaa.touches[0].clientY
        if(!using){return}
        if(eraserEnabled){
          context.clearRect(x-5,y-5,10,10)
        }else{
          var newPoint = {"x":x,"y":y}
          drawLine(lastPoint.x,lastPoint.y,newPoint.x,newPoint.y)
          lastPoint = newPoint
        }
      }
      canvas.ontouchend = function(){
        console.log('摸完了')
        using = false
      }
    }else{
      //非触屏设备
    canvas.onmousedown = function(aaa){
    var x = aaa.clientX 
    var y = aaa.clientY
    using = true
    if(eraserEnabled){
      context.clearRect(x-5,y-5,10,10)//矩形是把做左上角当起点画，故要减去一点才是矩形中间
    }else{
      lastPoint = {"x":x,"y":y}
    }
   }
  
   canvas.onmousemove = function(aaa){
    var x = aaa.clientX
    var y = aaa.clientY
    if(!using){return}
    if(eraserEnabled){
      context.clearRect(x,y,10,10)
    }else{
        var newPoint = {"x":x,"y":y}
        drawLine(lastPoint.x,lastPoint.y,newPoint.x,newPoint.y)
        lastPoint = newPoint
    }
  }
  
   canvas.onmouseup = function(aaa){
    using = false
   }
   }
  }
```
代码链接：http://js.jirengu.com/dimaj/2/edit?html,js  
(注：手机端适应的调试模拟必须将代码放在本地编辑器然后浏览器调试，而不可直接使用jsbin)
##  九、增加选色功能
```
<canvas id="xxx" width=300 height=300></canvas>
    <div id=actions class="actions">
        <span><i>Sage's</i> Canvas</span>
        <svg id=pen class="active icon">
            <use xlink:href="#icon-pen"></use>
        </svg>
        <svg id=eraser class="icon">
            <use xlink:href="#icon-eraser"></use>
        </svg>
        <svg id=save class="icon">
            <use xlink:href="#icon-save"></use>
        </svg>

       <!-- <button id=eraser>橡皮擦</button>
        <button id=brush>画笔</button>-->
    </div>
    <ol class="colors">
        <li id=red class="red"></li>
        <li id=yellow class="yellow"></li>
        <li id=blue class="blue"></li>
        <li id=black class="black"></li>
    </ol>
    <div class="divide"><hr></div>

/***********换颜色****************************/
red.onclick = function(){
    context.strokeStyle = 'red'
    red.classList.add('active')
    yellow.classList.remove('active')
    blue.classList.remove('active')
    black.classList.remove('active')
}
yellow.onclick = function(){
    context.strokeStyle = 'yellow'
    yellow.classList.add('active')
    red.classList.remove('active')
    blue.classList.remove('active')
    black.classList.remove('active')
}
blue.onclick = function(){
    context.strokeStyle = 'blue'
    blue.classList.add('active')
    red.classList.remove('active')
    yellow.classList.remove('active')
    black.classList.remove('active')
}
black.onclick = function(){
    context.strokeStyle = 'black'
    black.classList.add('active')
    red.classList.remove('active')
    yellow.classList.remove('active')
    blue.classList.remove('active')
}
/********************************************/
    ol{
        list-style: none;
    }
    .colors{
        position: fixed;
        top: 72px;
        left: 10px;
    }
    .colors > li{
        width: 20px;
        height: 20px;
        border: 1px solid #ddd;
        border-radius: 50%;
        box-shadow: 0 0 3px rgba(0,0,0,0.15);
        margin: 10px 0;
        transition: all 0.3s;
    }
    .colors > li.red{
        background: red;
    }
    .colors > li.yellow{
        background: yellow;
    }
    .colors > li.blue{
        background: blue;
    }
    .colors > li.black{
        background: #000;
    }
    .colors > li.active{
        box-shadow: 0 0 3px rgba(0,0,0,0.95);
        transform: scale(1.2);
    }
```
代码预览：http://js.jirengu.com/nosur/1/edit?html,js 
## 十、增加选画笔粗细功能
```
.sizes li{
    margin: 3px 3px;
}
.sizes .thin{
    height: 0;
    width: 40px;
    border-top: 2px solid black;
}
.sizes .thick{
    height: 0;
    width: 45px;
    border-top: 7px solid black;
}
```
```
<div class="fontweight clearfix">
        <ol class="sizes">
            <li id="thin" class="thin"></li>
            <li id="thick" class="thick"></li>
        </ol>
</div>
```
```
var lineWidth = 4

function drawLine(x1,y1,x2,y2){
    context.beginPath()
    //context.strokeStyle = 'black'
    context.moveTo(x1,y1) //起点 
    //context.lineWidth = 3
    context.lineWidth = lineWidth
    context.lineTo(x2,y2)  //终点
    context.stroke()
    context.closePath()
}
/***************** 自选线体粗细*****************/
thin.onclick = function(){
    lineWidth = 2
}
thick.onclick = function(){
    lineWidth = 7
}
/********************************************/
```
代码预览：https://jsbin.com/luqezi/edit?html,css,js,output
## 十一、增加清屏功能
```
/****************清屏功能**********************/
clear.onclick = function(){
    context.clearRect(0,0,yyy.width,yyy.height)
}
/********************************************/
<svg id=clear class="icon">
    <use xlink:href="#icon-bin"></use>
</svg>
```
## 十二、增加保存功能
```
/*************保存到本地的功能******************/
save.onclick = function(){
    var url = yyy.toDataURL("image/png")
    //console.log(url) //调试时可以看到是该文件的字符信息
    var a = document.createElement('a')
    document.body.appendChild(a)
    a.href = url
    a.download = '我的图画'
    a.target = '_blank'
    a.click()
}
/********************************************/
```
代码预览：https://jsbin.com/giraxuz/edit?html,js,output
