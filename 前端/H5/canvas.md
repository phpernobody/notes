## 创建画布

#### HTML: 
`<canvas id="canvas" width="500" height="500">您的浏览器不支持canvas</canvas>`

#### JS:

`
    var canvas = document.getElementById('canvas');
    if (canvas.getContext){
      var ctx = canvas.getContext('2d');
      // drawing code here
    } else {
      alert('浏览器太low，请及时更换');
    }
`

## 矩形绘制
#### 绘制填充矩形
`fillRect(x, y, width, height)`

#### 绘制矩形边框
`storkeRect(x, y, width, height)`

#### 清除矩形区域
`clearRect(x, y, width, height)`



## 路径绘制
#### 新建路径
`beginPath()`

#### 闭合路径
`closePath()`

#### 移动当前绘制点
`moveTo(x, y)`

#### 连接至某一点
`lineTo(x, y)`

#### 通过线条绘制路径轮廓
`stroke()`

#### 填充路径轮廓
`fill()`

#### 圆形路径

###### 画一个以（x,y）为圆心的以radius为半径的圆弧（圆），从startAngle开始到endAngle结束，按照anticlockwise给定的方向（默认为顺时针）来生成(radians=(Math.PI/180)*degrees)
`arc(x, y, radius, startAngle, endAngle, anticlockwise)`

###### 根据给定的控制点和半径画一段圆弧，再以直线连接两个控制点
`arcTo(x1, y1, x2, y2, radius)`
