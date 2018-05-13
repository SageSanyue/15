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
