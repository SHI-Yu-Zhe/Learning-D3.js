[TOC]

# Data-Driven Documents

D3（Data-Driven Documents）是一种用于翻译带数据的文件对象模型（DOM）作用于特定嵌入空间的语言，是一种拥有良好兼容性、可调试性和运行表现的语言。比起一种可视化框架，它更像是一个可视化内核，其逻辑为将数据与基础元素绑定。它的最大贡献是能同时集成网页内容、网页美学、用户交互和向量图表示的功能，原本这些要分别通过HTML、CSS、JS和SVG来实现。

## 基础逻辑

D3的元操作数是selection，这是从当前文件查询得到的筛选集。

一切运算符作用于selection，改变着内容。

Data joins 将数据绑定至元素，为作用于数据的函数运算符赋能。

enter/exit创造或销毁元素及相关数据。						

在运算符按照默认设定运算的瞬间，transition动画将样式（styles）和属性（attributes）随时间变化平滑地插入。

一种被称为event handler的运算符能对输入的数据作出回应并提供交互功能。

技术路线：

Source Data 源数据-> (data) -> Variables 变量集->（var set）-> Algebra 代数-> (var set) -> Scale 规模操作-> (var set) -> Statistics 统计数据-> (Graph) -> Geometry 几何表示-> (Graph) -> Coordinates 坐标表示 -> (Graph) -> Aesthetics 美学-> (Graph) -> Renderer 渲染

对应d3格式:

```js
datad3.json(); -> d3.nest(); -> d3.scale(); -> d3.layout(); -> d3.selection().append(); -> d3.selection().attr();
```

正由于d3函数的返回值总是选择的元素，因此能够形成传递闭包，可以进行链式调用。个人认为这与Java的链式调用不完全一样，Java支持链式调用的许多函数为空返回值。
d3 chain:(以圆为例)

```javascript
svg.append('circle') //在向量图中扩展一个圆
	.attr('fill','red')  //填充红色
	.attr('r',50)  //设置半径、圆心横纵坐标
	.attr('cx',2)
	.attr('cy',4)
```

### selection

按标签选择("tag")
按类选择(".class")
按独一无二的身份号码选择("#id")
按属性选择("[name=value]")
按容器性质选择("parent child")
按邻接关系选择("before ~ after")

select仅仅获取满足条件的第一个元素，而selectAll获取满足条件的所有元素。
select也支持不同层级的搜索，后一个select总是在由前一个select确定的域中搜索得到。例如，选择所有的("p")并选择每个元素中第一个("b")可以用如下代码表示：

```javascript
d3.selectAll("p").select("b");
```

对于每个元素，其他任意的运算符均可进行操作：
属性attr()
样式style()
性质property()
HTML html()
文本 text()

### data

拥有一组任意值的向量，元素数据类型可以是数值，字符串或对象。数据与对应元素的下标保持一致。



> 如图所示，enter/update/exit运算符是针对数据在结点上的绑定。若数据被绑定到返回值为空的selection上，则通过enter增加新结点来绑定数据，而这些结点接下来可以使用append/insert进行初始化；若数据被绑定到存在的结点上，则通过exit移除结点上原有数据的绑定。
> 设D为数据，N为结点，则enter 操作为D->N,exit操作为N->D,update操作为D<->N。这样的三种运算符构成了数据的生命周期。![1565099424467](/home/shi/.config/Typora/typora-user-images/1565099424467.png)

数据可以使用sort进行排序，或filter进行筛选。

### animation & interaction

d3支持事件监听器(event listeners),这种机制通过回调函数完成在用户出现一些特定操作时作用于特定元素。on 运算符就是对于一些原生的事件进行操作的。

动态渐变可以由selection派生出来，使用transition运算符。渐变在同一时刻暴露属性、样式，但随着时间变化，它从当前时刻连续插值到目标时刻。插值可以有多种数据类型：数值，嵌入数值的字符串，RGB或HSL颜色，任意嵌套的向量或对象。

由于不同元素执行回调函数完成时间不同，D3自动管理不同时间的渐变，保证每个元素有效完成动作。

### model

D3提供大量可复用的模型，用于解决一些同类型问题。

d3.svg模型用于绘制基础图表。

布局模型支持可复用可视化技术，通过生成抽象数据结构类型。

交互技术是通过行为来实现复用的。

## 安装D3

### 方式一：本地安装

在https://d3js.org/下载安装包 -> 解压，将d3.v3.js复制到工程文件所在目录，使用以下代码加入工程文件的.html文件：

```html
<head>
	<script src = "../d3.min.js"></script>
</head>
```

### 方式二：使用内容分发网络(Content Deliver Network,CDN)

使用以下代码加入工程文件的.html文件：

```html
<head>
	<script src="https://d3js.org/d3.v5.min.js"></script>
</head>
```

实验测试：使用以下命令启动服务器

```
shi@shiyuzhe:~$ python -m SimpleHTTPServer
```

打开浏览器，在默认的localhost:8000端口查看。

注意：更改一次代码后，要reload一次才能查看最新效果。

## D3-DOM Selection 文件对象选择

### 基础例子：

```html
<body>
    <p>First Paragraph</p>
    <p>Second Paragraph</p>
    <script>
        d3.select("p").style("color","blue");
    </script>
</body>
```

![1565145511464](/home/shi/.config/Typora/typora-user-images/1565145511464.png)

### 通过ID选择：

```html
<body>
    <p id="p1">First Paragraph</p>
    <p id="p2">Second Paragraph</p>
    <script>
        d3.select("p").style("color","blue");
        d3.select("#p2").style("color","green");
    </script>
</body>
```

![1565145669210](/home/shi/.config/Typora/typora-user-images/1565145669210.png)

### 全部选择：

```html
<body>
    <p id="p1" class="cc">First Paragraph</p>
    <p id="p2">Second Paragraph</p>
    <p id="p3" class="cc">Third Paragraph</p>
    <script>
        d3.select("p").style("color","blue");
        d3.select("#p2").style("color","green");
        d3.selectAll(".cc").style("background-color","red");
    </script>
</body>
```

![1565146565656](/home/shi/.config/Typora/typora-user-images/1565146565656.png)

### 选择嵌套数据（如表格）：

```html
<body>
    <table>
        <tr>
            <td>
                One
            </td>
            <td>
                Two
            </td>
        </tr>
        <tr>
            <td>
                Three
            </td>
            <td>
                Four
            </td>
        </tr>
    </table>
    <script>
        d3.select("tr").selectAll("td").style("background-color","yellow");
    </script>
</body>

```

![1565146643502](/home/shi/.config/Typora/typora-user-images/1565146643502.png)

## D3-DOM Manipulation 文件对象操作

### 设置内部HTML：

```html
<body>
    <p id="p1" class="cc">First Paragraph</p>
    <script>
        d3.select("#p1").html("<span>This was added in HTML</span>");
    </script>
</body>
```

![1565147095113](/home/shi/.config/Typora/typora-user-images/1565147095113.png)

### 设置属性（attribute）：

```HTML
<head>
    <meta charset="UTF-8">
    <script src = "d3.js"></script>
    <title>maniTest</title>
    <style>
        .highlight{
            color:yellowgreen
        }
    </style>
</head>
<body>
    <p id="p1" class="cc">First Paragraph</p>
    <script>
        d3.select("#p1").html("<span>This was added in HTML</span>");
        d3.select(".cc").attr("class","highlight");
    </script>
</body>
```

![1565147374097](/home/shi/.config/Typora/typora-user-images/1565147374097.png)

### 设置属性（property）：

```HTML
<body>
    <p>Brahms</label><input type="checkbox" /></p>
    <p>Schumann</label><input type="checkbox" /></p>
    <script>
        d3.select("input").property("checked",true);
    </script>
</body>
```

![1565147759397](/home/shi/.config/Typora/typora-user-images/1565147759397.png)

### 添加、删除类：

```HTML
<body>
    <p id="p1" class="cc">First Paragraph</p>
    <script>
        d3.select("#p1").html("<span>This was added in HTML</span>");
        d3.select(".cc").attr("class","highlight");
        d3.select("p").classed("highlight",false);
        //if class exists then remove the class from the element
    </script>
</body>
```

![1565147972367](/home/shi/.config/Typora/typora-user-images/1565147972367.png)

## D3-链式调用

### 链式调用扩展数据：

```html
<body>
<script>
    d3.select("body") //选择body元素，返回引用
        .append("p")  //为body元素扩展出子级"paragraph"
        .text("Guten tag!");  //为"paragraph"添加内容
</script>
</body>
```

![1565151238811](/home/shi/.config/Typora/typora-user-images/1565151238811.png)

## Data函数

在链式调用中，方法支持以匿名函数作为参数传入，实现自定义的逻辑功能。

形式如下：

```javascript
.text(function(d,i){ // "d" stands for data, "i" stands for index
	console.log(d);
	console.log(i);
	console.log(this);// "this" stands for a referrence of the current DOM object
})
```

```html
<body>
    <p></p>
    <p></p>
    <p></p>
    <script>
        var data = [1,2,3];
        var paragraph = d3.select("body")
            .selectAll("p")
            .data(data)
            .text((d,i)=>{
                console.log("d:"+d);
                console.log("i:"+i);
                console.log("this:"+this);
                return d;
            })
    </script>
</body>
```

![1565158033379](/home/shi/.config/Typora/typora-user-images/1565158033379.png)

扩展：动态调用，通过匿名函数动态决定属性。

```html
<body>
    <p>Brahms: Konzert fuer klavier und ochester Nr.2 </p>
    <p>Schumann: Waldszene, Op.82</p>
    <script>
        d3.selectAll("p").style("color",(d,i)=>{
            var text = this.innerText;
            if(text.indexOf("Brahms")>=0)  return "red";
            else if(text.indexOf("Schumann")>=0)  return "yellow";	// 通过关键字是否存在，即函数返回值来决定文本颜色
        });
    </script>
</body>
```

## 事件处理

### 内置事件

通过一行代码实现事件监听器：

```javascript
d3.selection.on(type[,listener[,capture]]);
```

on()方法有三个参数，第一个是事件类型，例如"click","mouseover","mouseout"；第二个是可自定义的回调函数，在事件发生时执行；第三个参数不是必需的，它捕捉特定的flag。

总结：selection.on() 用于添加或移除监视"click"/"mouseover"等事件的监听器；selection.dispatch() 用于捕捉事件，且类型名就是事件名，对应的监听器就是对应监听器；d3.event 是用于获取事件域（如timestamp）或事件方法（如preventDefalut）的事件对象；d3.mouse(container) 用于获取鼠标所在位置在特定DOM元素的横纵坐标；d3.touch() 用于获取容器中鼠标接触坐标。

实现一个鼠标移动到蓝色区域内就会变成橘黄色且展示鼠标位置坐标的功能：

```HTML
<body>
<div></div>
<script>
    d3.selectAll("div")
    .on("mouseover",()=>{
        d3.select(this)
            .style("background-color","orange");
        console.log(d3.event); // get current event info
        console.log(d3.mouse(this)); // get x & y co-ordinates
    })
    .on("mouseout",()=>{
        d3.select(this)
            .style("background-color","steelblue"); // the background color restitutes
    })
</script>
</body>
```

![1565185385838](/home/shi/.config/Typora/typora-user-images/1565185385838.png)

## D3动画

selection.transition() 方法为选定的元素使用过渡。

transition.duration() 为每个元素确定动作持续时间；transition.ease() 为每个元素确定缓冲动作方式，例如：linear线性，elastic弹性，bounce弹跳式；transition.delay() 为每个元素确定动画延迟时间。

实现一个简单的变色图片：

```HTML
<body>
<div id="container"></div>
<script>
    d3.select("#container")
    .transition()
    .duration(1000)
    .style("background-color","red");
</script>
</body>
```

![1565185440411](/home/shi/.config/Typora/typora-user-images/1565185466716.png)

实现一个线性缓冲：第一个柱形图有20px长高至100px，用时2000毫秒；2000毫秒后第二个柱形图也完成相同的动作。

```html
<body>
    <script>
        var svg = d3.select("body")
            .append("svg")
            .attr("width",500)
            .attr("height",500);
        var bar1 = svg.append("rect")
            .attr("fill","blue")
            .attr("y",20)
            .attr("x",100)
            .attr("width",10)
            .attr("height",20);
        var bar2 = svg.append("rect")
            .attr("fill","blue")
            .attr("y",20)
            .attr("x",120)
            .attr("width",10)
            .attr("height",20); // define two bars
        update();
        function update(){
            bar1.transition()
                .ease(d3.easeLinear)
                .duration(2000)
                .attr("height",100);
            bar2.transition()
                .ease(d3.easeLinear)
                .duration(2000)
                .delay(2000)
                .attr("height",100);
        } // define the transition 
    </script>
</body>
```

![1565186211085](/home/shi/.config/Typora/typora-user-images/1565186211085.png)

分析代码可以看到，要使用动画，首先要调用transition()方法，声明动画的开始，这一点与后面的绑定数据要使用enter()有异曲同工之妙；随后调用调用的是变化方式、变化方式持续时间duration()；可选的方法是延迟时间delay()，其延迟是相对于没有设定延迟时间的动画的。

## 绑定数据

重要数据操作方法：

data() 将数据连接至选定元素，其支持的数据类型是向量，元素数据类型包括CSV，TSV，XML，JSON等；enter() 创建一个元素之前不存在的selection；exit() 移除结点，并将它们添加之即将被从DOM移除的元素上；datum() 在不生成连接的情况下将数据注入所选元素。

最简单的数据连接：

```html
<body>
<p></p>
<p></p>
<p></p>
<script>
    var myData = ["Brahms","Schumann","Mendelssohn"]; // The data must be a vector or an array!
    var p = d3.select("body")
    .selectAll("p")
    .data(myData)
    .text(function(d,i){
        return d;
    })
</script>
</body>
```

![1565189461462](/home/shi/.config/Typora/typora-user-images/1565189461462.png)

接下来就可以观察enter()与append()的区别了。append()只能在已有的结点中连接数据，如上例子所示，我们数据数组恰有三个元素，而段落恰有三个，这使得三个元素均能正确连接数据；若数组中有超过三个元素，段落只有三个，则最终输出与上图一样：多余的元素由于没有足够的结点而被忽略了。enter则不同了，它可以创建之前不存在的结点，直到所有数据元素都与结点完成了连接。以此类推，exit()与remove()的关系也如出一辙。

```HTML
<body>
    <script>
        var myData = ["Webber\n","Suppe\n","Piefuke\n"];
        var p = d3.select("body")
            .selectAll("span")
            .data(myData)
            .enter()
            .append("span")
            .text(function (d,i){
                return d;
            })
    </script>
</body>
```

![1565190361298](/home/shi/.config/Typora/typora-user-images/1565190361298.png)

datum() 与 data() 的区别主要在于，前者将数据直接绑定到元素上，使其成为元素的属性，而不是向后者一样建立数据与元素的连接。

## 载入数据

重要的方法：对应数据类型名即为方法名。

d3.csv(); d3.tsv(); d3.json(); d3.xml() 支持载入的数据类型对应其名称，分别为csv,tsv,json,xml 。其标准格式如下：

```javascript
d3.csv(url[,row,call_back_funcion]);
```

第一个参数是.csv文件的URL或某一可以返回.csv文件的网络编程接口或网络服务的URL；第二个参数不必需，是一个改变表现方式的函数；第三个参数不必需，是一个回调函数，在数据加载完毕时执行。

当然，这三个参数还可以拆分成一组链式调用：

```javascript
d3.csv(url)
.row(convert_function(){})
.get(call_back_function(){});
```

另外三个方法tsv/json/xml标准格式如下：

```javascript
d3.name(url,call_back_function(){});
```

下面的例子加载了一个json文件，并在回调函数中添加error参数作为异常处理。

```HTML
<body>
    <script>
        d3.json("./user.json",function(error,data){
            if(error)
                return console.warn(error);
            d3.select("body")
                .selectAll("p")
                .data(data) // bind data to elements which don't exist at the time
                .enter() // create new elements and return references
                .append("p") // create new nodes referenced to new elements
                .text(function(d){
                    return d.name+", "+d.location;
                })
        });
    </script>
</body>
```

需要特别注意的是，第二个参数，即回调函数的逻辑是在数据加载成功后开始执行，因此要将数据驱动的所有操作放入函数体之中，也就是说，在数据未能成功加载时，不会有任何失去数据控制的操作出现。而在后面的代码中出现的data变量看似没有经过定义，但它是由匿名回调函数传递进来的形式参数，它就是一个基于对象元素的向量。

## 创建可扩展向量图(Scalable Vector Graphics)

SVG是一种给予文本的图片，其结构与HTML相似，它位于DOM之中，它的性质属性要通过attributes来确定，它的任何图形必需有相对于原点的精确位置。

### 定义一个矩形：

一个顶点的坐标，长和宽，默认填充颜色为黑色。

```html
<body>
    <svg width="500" height="500">
        <rect x="0" y="0" height="100" width="200" ></rect>
        // 以及任何svg元素
    </svg>
</body>
```

接下来用D3实现上面的矩形：

```html
<body>
    <script>
        var svg = d3.select("body")　// create a svg element
            .append("svg")
            .attr("height",500)
            .attr("weight",500);
        svg.append("rect")　// create a rectangle element of svg
            .attr("x",0)
            .attr("y",0)
            .attr("width",100)
            .attr("height",200);
    </script>
</body>
```

![1565227084409](/home/shi/.config/Typora/typora-user-images/1565227084409.png)

### 定义一条直线：

两个端点的坐标，要通过stroke确定颜色，否则默认为白色。

```html
<line x1="500" y1="500" x2="300" x2="400" stroke="black"></line>
```

接下来使用D3实现上面的SVG直线:

```html
<body>
    <script>
        var width = 500;
        var height = 500;
        var svg = d3.select("body") // create a svg element
            .append("svg")
            .attr("height",height)
            .attr("width",width);
        svg.append("line") // create a line element of svg
            .attr("x1",500)
            .attr("y1",500)
            .attr("x2",300)
            .attr("y2",400)
            .attr("stroke","black");
    </script>
</body>
```

![1565227813671](/home/shi/.config/Typora/typora-user-images/1565227813671.png)

### 定义一个圆：

圆心坐标，半径，默认填充黑色。

```html
<svg width="500" height="500">
        <circle r="50" cx="250" cy="50"/>
</svg>
```

接下来用D3实现：

```html
<body>
    <script>
        var svg = d3.select("body")
            .append("svg")
            .attr("height",500)
            .attr("width",500);
        svg.append("circle")
            .attr("r",50)
            .attr("cx",250)
            .attr("cy",50);
    </script>
</body>
```

![1565229269723](/home/shi/.config/Typora/typora-user-images/1565229269723.png)

### 定义一个椭圆：

圆心坐标，长短轴，默认填充黑色。

```html
<svg height="500" width="500">
        <ellipse rx="100" ry="25" cx="250" cy="25"/>
</svg>
```

下面用D3实现：

```html
<body>
    <script>
        var svg = d3.select("body")
            .append("svg")
            .attr("width",500)
            .attr("height",500);
        svg.append("ellipse")
            .attr("rx",100)
            .attr("ry",25)
            .attr("cx",400)
            .attr("cy",200)
            .attr("fill","steelblue"); // change the default filling color
    </script>
</body>
```

![1565229613263](/home/shi/.config/Typora/typora-user-images/1565229613263.png)

### 添加文本：

文本位置坐标，内容，文字默认黑色。

```html
<svg width="500" height="500">
        <text x="300" y="300">I am the text!</text>
</svg>
```

下面用D3实现：

```html
<body>
    <script>
        var svg = d3.select("body")
            .append("svg")
            .attr("height",500)
            .attr("width",500);
        // create a group element to gather ellipse with text
        var g = svg.append("g")
            .attr("transform",function(d,i){
                return "translate(0,0)";
            })
        g.append("ellipse")
            .attr("cx",250)
            .attr("cy",50)
            .attr("rx",150)
            .attr("ry",50);
        g.append("text")
            .attr("x",250)
            .attr("y",50)
            .attr("stroke","red")
            .text("This is an ellipse!");
    </script>
</body>
```

![1565230373725](/home/shi/.config/Typora/typora-user-images/1565230373725.png)

观察图片和代码我们可以知道：文本的坐标指的是其首字母型心的位置。
分析代码，我们在创建svg元素之后创建了group "g" 元素，目的是将后面的元素椭圆和文本直接由group扩展得到，使它们形成整体。

### 一些常用属性：

它们使可视化更漂亮！

"fill"——填充颜色； "stroke"——线段、字体颜色； "stroke-width"——线段或边界的宽上色； "opacity"——透明度，[0,1]； "font-family"——字体； "font-size"——字号。

对上个例子加入一些新属性；

```html
<body>
    <script>
        var svg = d3.select("body")
            .append("svg")
            .attr("height",500)
            .attr("width",500);
        // create a group element to gather ellipse with text
        var g = svg.append("g")
            .attr("transform",function(d,i){
                return "trdanslate(0,0)";
            })
        g.append("ellipse")
            .attr("cx",250)
            .attr("cy",50)
            .attr("rx",150)
            .attr("ry",50)
            .attr("fill","steelblue")
            .attr("opacity",0.5)
            .attr("stroke-width","orange");
        g.append("text")
            .attr("x",250)
            .attr("y",50)
            .attr("stroke","red")
            .attr("font-family","sans-serif")
            .text("This is an ellipse!");
    </script>
</body>
```

![1565231137425](/home/shi/.config/Typora/typora-user-images/1565231137425.png)

## 创建图表

### 柱状图表：

```html
<html lang="en">
<head>
    <meta charset="UTF-8">
    <script src = "d3.js"></script>
    <style>
        svg rect{
            fill:orange;
        }
        svg text{
            fill:white;
            font-family:sans-serif;
            text-anchor:end;　// keep the text stay at right
        }
    </style>
    <title>svgBarChart</title>
</head>
<body>
    <script>
        var data = [5,10,20];
        var scaleFactor = 10, // the scale factor of data is 10,
            // they need to be scaled to a pixel value to be visible
            width = 200,
            barHeight = 20;
        var graph = d3.select("body") // create a graph element
            .append("svg")
            .attr("width",width)
            .attr("height",barHeight*data.length);
        var bar = graph.selectAll("g") // append a group element
            // and create a bar element
            .data(data)
            .enter()
            .append("g")
            .attr("transform",function(d,i){
                return "translate(0,"+i*barHeight+")";
            })
        bar.append("rect")
            .attr("width",function(d){
                return d*scaleFactor;
            })
            .attr("height",barHeight-1); // append the bar, exclude the original bar
        bar.append("text")
            .attr("x",function(d){
                return d*scaleFactor;
            })
            .attr("y",barHeight/2) // keep the text stay middle of the end of bar
            .attr("dy",".35em")
            .text(function(d){
                return d;
            });
    </script>
</body>
</html>
```

![1565233573350](/home/shi/.config/Typora/typora-user-images/1565233573350.png)

分析代码，可以了解柱状图的工作原理：首先扩展一个svg图区域，其宽度固定，而高度不固定，故长度由以下公式决定，其中d.length是数据向量的维度数量：
$$
l = d.length*barHeight
$$
预定义的是每个柱形图的高度，这是一个常量值，而"transform"属性中的公式
$$
ｙ=i*barHeight
$$
正是利用这一点计算出每个柱形图矩形的顶点坐标，i是数据元素在向量中的下标，因此，这三个柱形图横坐标均为０，第一个柱形图的纵坐标为０，第二个为２０，第三个为４０。
每个柱形图的长度是变量，这与它们对应的数据值有关。由于我们默认柱形图由一个个小矩形组成，因此之前定义的放大倍数因子即为柱形图步进，这是为了使原本很小的数据得到更多的像素点表示，能更好地可视化。对应公式
$$
barLength=d*scaleFactor
$$
总结一下，与数据有关的变量有三个：svg图的高度由数据向量维度数量×柱状图高度决定，在定义svg时确定；每个柱状图顶点纵坐标由数据元素下标×柱状图高度决定，在定义bar变量时在"transform"属性中确定；每个柱状图长度由数据大小×像素放大因子决定，在将bar扩展为矩形并添加文本时确定。

### 圆形元素图：

```html
<body>
    <script>
        var data = [10, 15, 20, 25, 30];
        var colors = ['#ffffcc','#c2e699','#78c679','#31a354','#006837'];
        var svg = d3.select("body")
            .append("svg")
            .attr("height",500)
            .attr("width",500);
        var g = svg.selectAll("g")
            .data(data)
            .enter()
            .append("g")
            .attr("transform",function(d,i){
                return "translate(0,0)";
            });
        g.append("circle")
            .attr("cx",function(d,i){
                return 50+i*100;
            })
            .attr("cy",100)
            .attr("r",function(d,i){
                return d*1.5;
            })
            .attr("fill",function(d,i){
                return colors[i];
            });
        g.append("text")
            .attr("x",function(d,i){
                return 55+i*100;
            })
            .attr("y",105)
            .attr("stroke","teal")
            .attr("font-size","12px")
            .attr("font-family","sans-serif")
            .text(function(d){
                return d;
            })
    </script>
</body>
```

![1565245612347](/home/shi/.config/Typora/typora-user-images/1565245612347.png)

通过代码我们发现，定义这种大小与数据大小成正比的圆逻辑比柱状图还要简单。在相关文献中，作者专门分析了使用半径还是面积来表征大小，最终研究结论说明使用半径长度与数据大小成正比的可视化编码更符合人类认知。

分析代码，我们注意到这里圆心坐标是需要通过公式计算的：
$$
x=50+i*100
$$
其意义为每个圆间隔为100px，而对半径也进行了恒等映射，半径均是数据放大1.5倍得到。其余部分与定义柱状图表大同小异。

## 数据映射(Scales)

映射使得数据可视化更加符合人类认知，正如我们在上一个例子中所做的那样。

连续映射：d3.scaleLinear()线性映射，将输入域直接映射到输出域； d3.scaleIdentity() 线性映射，输入输出有同种数据类型；d3.scaleTime()　线性映射，输入时间，输出是数值； d3.scaleLog() 对数映射；d3.scaleSqrt() 平方根映射；d3.scalePow()指数映射。

顺序映射：d3.scaleSequential()　建立输出域被插值函数固定的顺序映射。

量化映射：d3.scaleQuantize() 建立离散域的输出。

数位映射：d3.scaleQuantile()　建立从样本输入域到离散域输出的数位映射。

阈值映射：d3.scaleThreshold() 建立从任意输入域到离散域输出的阈值映射。

Band映射： d3.scaleBand()　建立类似叙述尺度的映射，只要输出域不是连续且数值的。（例如：年份的映射轴）

点映射：d3.scalePoint()　建立点映射。

序数映射：d3.scaleOrdinal()　建立从包括字母域在内的输入域到离散数值输出域的映射。

在下面的例子中，我们将使用线性映射将较大的输入域映射到较小的输出域。这将在柱状图宽度和文本横坐标的计算中代替我们原先的像素放大因子。

```html
<body>
    <script>
        var data = [100,250,360,140,670,890,1000];
        var barHeight = 20;
        var scale = d3.scaleLinear()
            .domain([d3.min(data),d3.max(data)])
            .range([50,500]);
        var svg = d3.select("body")
            .append("svg")
            .attr("height",data.length*barHeight)
            .attr("width",500);
        var g = svg.selectAll("g")
            .data(data)
            .enter()
            .append("g")
            .attr("transform",function(d,i){
                return "translate(0,"+i*barHeight+")";
            });
        g.append("rect")
            .attr("height",barHeight-1)
            .attr("width",function(d,i){
                return scale(d);
            });
        g.append("text")
            .attr("x",function(d){
                return scale(d);
            })
            .attr("y",barHeight/2)
            .attr("dy",".35em")
            .text(function(d){
                return d;
            });
    </script>
</body>
```

![1565255083318](/home/shi/.config/Typora/typora-user-images/1565255083318.png)

![1565255119211](/home/shi/.config/Typora/typora-user-images/1565255119211.png)

## 轴

常用方法：d3.axisTop()创建顶部水平轴； d3.axisRight()创建右侧垂直轴； d3.axisBottom()创建底部水平轴； d3.axisLeft()创建左侧垂直轴。 

![1565255214460](/home/shi/.config/Typora/typora-user-images/1565255214460.png)

加入一条水平轴：

```html
<body>
    <script>
        var data = [10,15,20,15,30];
        var svg = d3.select("body")
            .append("svg")
            .attr("width",400)
            .attr("height",400);
        var scale = d3.scaleLinear()
            .domain([d3.min(data),d3.max(data)])
            .range([0,300]);
        var x_axis = d3.axisBottom()
            .scale(scale);
        svg.append("g")
            .call(x_axis);
    </script>
</body>
```

![1565255748749](/home/shi/.config/Typora/typora-user-images/1565255748749.png)

分析代码，这里我们是将较小输入域映射到了较大输出域，与上一个例子恰好相反。在最后，通过svg扩展出group元素，并call了之前定义的x轴。

添加一条纵轴：

```html
<body>
    <script>
        var data = [10,15,20,15,30];
        var svg = d3.select("body")
            .append("svg")
            .attr("width",400)
            .attr("height",400);
        var scale = d3.scaleLinear()
            .domain([d3.min(data),d3.max(data)])
            .range([5,300]);
        var x_axis = d3.axisBottom()
            .scale(scale);
        var y_axis = d3.axisRight()
            .scale(scale);
        svg.append("g")
            .call(x_axis);
        svg.append("g")
            .call(y_axis);
    </script>
</body>
```

![1565256271770](/home/shi/.config/Typora/typora-user-images/1565256271770.png)

将两个轴对齐，形成一个完整坐标系：

```html
<body>
    <script>
        var data = [10,15,20,25,30];
        var height = 400;
        va看出，d3.scaleOrdinar svg = d3.select("body")
            .append("svg")
            .attr("width",400)
            .attr("height",height);
        var xscale = d3.scaleLinear()
            .domain([0,d3.max(data)])
            .range([0,300]);
        var yscale = d3.scaleLinear()
            .domain([0,d3.max(data)])
            .range([height/2,0])
        var x_axis = d3.axisBottom()
            .scale(xscale);
        var y_axis = d3.axisLeft()
            .scale(yscale);
        svg.append("g")
            .attr("transform","translate(50,10)")
            .call(y_axis);
        var xAxisTranslate = height/2+10; // to make x and y axis well-organized
        svg.append("g")
            .attr("transform","translate(50,"+xAxisTranslate+")")
            .call(x_axis);
    </script>
</body>
```

![1565259343964](/home/shi/.config/Typora/typora-user-images/1565259343964.png)

分析代码，我们可以发现：为了使x,y轴恰好在零点位置相交，我们对y进行了负相关映射，使其起点处对应最大值，终点处对应最小值；而计算式
$$
translate=height/2+10
$$
计算x轴初始纵坐标位置，恰好将y轴长度加上y初始纵坐标位置，式两轴得以在原点相交。

## 创建柱形图

之前学习了柱状图和坐标轴的创建方法，我们可以使用这两个元素来创建柱形图。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <script src = "d3.js"></script>
    <style>
        .bar{
            fill:steelblue;
        }
    </style>
    <title>barChart</title>
</head>
<body>
    <svg height="500" width="600"></svg>
    <script>
        var margin = 200; // set global variable margin for further adjustment
        var svg = d3.select("svg"),
            width = svg.attr("width")-margin,
            height = svg.attr("height")-margin;
        svg.append("text")
            .attr("transform","translate(100,0)")
            .attr("x",50)
            .attr("y",50)
            .attr("font-size","24px")
            .text("XYZ Foods Stock Price"); // add more attributes to svg
        var xScale = d3.scaleBand().range([0,width]).padding(0.4);
         // set padding to obtain some space between bars
        var yScale = d3.scaleLinear().range([height,0]);
        var g = svg.append("g")
            .attr("transform","translate(100,100)");
         // add a transform to position our graph with some margin
        d3.csv("XYZ.csv",function(error,data){
            if (error)  throw error;
            xScale.domain(data.map(function(d){
                return d.year;
            })); // the method map returns an array which matches the parameter type of domain()
            yScale.domain([0,d3.max(data,function(d){
                return d.value;
            })]);
            g.append("g")
                .attr("transform","translate(0,"+height+")")
                .call(d3.axisBottom(xScale))
                .append("text")
                .attr("x",width-100)
                .attr("y",height-250)
                .attr("text-anchor","end")
                .attr("stroke","black")
                .text("Year"); // add x axis
            g.append("g")
                .call(d3.axisLeft(yScale).tickFormat(function(d){
                    return "$"+d; // add "$" symbol as tick format
                }).ticks(10))
                .append("text")
                .attr("transform","rotate(-90)") // to make a horizon string rotate for pi/2
                .attr("y",6)
                .attr("dy","-5.1em")
                .attr("text-anchor","end")
                .attr("stroke","black")
                .text("Stock Price");  // add y axis
            g.selectAll(".bar")
                .data(data)
                .enter()
                .append("rect")
                .attr("class","bar") //append bar-classed elements
                .attr("x",function(d){return xScale(d.year)}) // x ordinal of bar
                .attr("y",function(d){return yScale(d.value)}) // y ordinal of bar
                .attr("width",xScale.bandwidth()) // bar with space between
                .attr("height",function(d){
                    return height-yScale(d.value); // calculate height of bar from top
                });
        }); // load data and try the errors

    </script>
</body>
</html>
```

## 动态柱形图

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <script src = "d3.js"></script>
    <style>
        .bar{
            fill:steelblue;
        }
        .highlight{
            fill:orange;
        }
    </style>
    <title>AnimationBarChart</title>
</head>
<body>
<svg height="500" width="600"></svg>
<script>
    var margin = 200; // set global variable margin for further adjustment
    var svg = d3.select("svg"),
        width = svg.attr("width")-margin,
        height = svg.attr("height")-margin;
    svg.append("text")
        .attr("transform","translate(100,0)")
        .attr("x",50)
        .attr("y",50)
        .attr("font-size","24px")
        .text("XYZ Foods Stock Price"); // add more attributes to svg
    var xScale = d3.scaleBand().range([0,width]).padding(0.4);
    // set padding to obtain some space between bars
    var yScale = d3.scaleLinear().range([height,0]);
    var g = svg.append("g")
        .attr("transform","translate(100,100)");
    // add a transform to position our graph with some margin
    d3.csv("XYZ.csv",function(error,data){
        if (error)  throw error;
        xScale.domain(data.map(function(d){
            return d.year;
        })); // the method map returns an array which matches the parameter type of domain()
        yScale.domain([0,d3.max(data,function(d){
            return d.value;
        })]);

        g.append("g")
            .attr("transform","translate(0,"+height+")")
            .call(d3.axisBottom(xScale))
            .append("text")
            .attr("x",width-100)
            .attr("y",height-250)
            .attr("text-anchor","end")
            .attr("stroke","black")
            .text("Year"); // add x axis
        g.append("g")
            .call(d3.axisLeft(yScale).tickFormat(function(d){
                return "$"+d; // add "$" symbol as tick format
            }).ticks(10))
            .append("text")
            .attr("transform","rotate(-90)") // to make a horizon string rotate for pi/2
            .attr("y",6)
            .attr("dy","-5.1em")
            .attr("text-anchor","end")
            .attr("stroke","black")
            .text("Stroke Price");  // add y axis
        g.selectAll(".bar")
            .data(data)
            .enter()
            .append("rect")
            .attr("class","bar")//append bar-classed elements
            .on("mouseover",onMouseOver)//call the custom function when mouse move on
            .on("mouseout",onMouseOut)//call the custom function when mouse move out
            .attr("x",function(d){return xScale(d.year)}) // x ordinal of bar
            .attr("y",function(d){return yScale(d.value)}) // y ordinal of bar
            .attr("width",xScale.bandwidth())
            .transition()
            .ease(d3.easeLinear)
            .duration(400)// bar with space between
            .attr("height",function(d){
                return height-yScale(d.value); // calculate height of bar from top
            });
    }); // load data, create scales and try the errors

    function onMouseOver(d,i){
        d3.select(this).attr("class","highlight"); //make the bar highlighted
        d3.select(this)
            .transition()
            .duration(400)
            .attr("width",xScale.bandwidth()+5) //make the bar wider
            .attr("y",function(d){
                return yScale(d.value)-10;
            })
            .attr("height",function(d){
                return yScale(d.value)+10; //make the bar higher
            });
        g.append("text")
            .attr("class","val")
            .attr("x",function(d){
                return xScale(d.year);
            })
            .attr("y",function(d){
                return yScale(d.value)-15;
            })
            .text(function(d){
                return ['$'+d.value];//add a text tip on the mouse with value
            });
    }
    function onMouseOut(d,i){
        d3.select(this).attr("class","bar");//make the bar back to ordinary
        d3.select(this)
            .transition()
            .duration(400)
            .attr("width",xScale.bandwidth())
            .attr("y",function(d){
                return yScale(d.value);
            })
            .attr("height",function(d){
                return height-yScale(d.value);
            });
        d3.selectAll(".val")
            .remove(); // remove class val as mouse move out
    }
</script>
</body>
</html>
```



## 创建饼状图

SVG Path用于绘制在SVG中的路径。

例子：绘制一个闭合环路区域。

```html
<body>
    <svg height="210" width="400">
        <path d="M150 0 L75 200 L225 200 Z"/>
    </svg>
</body>
```

![1565484134050](/home/shi/.config/Typora/typora-user-images/1565484134050.png)

这条路径定义了顶角坐标和两个底角坐标，形成一个三角形闭合回路。默认填充黑色。

d3.scaleOrdinal()是坐标映射方法，将一个空定义域映射到特定的陪域。下面的例子就是从一个向量中映射。

```html
<body>
    <script>
        var color = d3.scaleOrdinal(['#4daf4a','#377素eb8','#ff7f00','#984ea3','#e41a1c']);
        console.log(color(0));
        console.log(color(1));
        console.log(c<body>
    <svg width="300" height="200"></svg>
    <script>
        var data = [2,4,8,10];
        var svg = d3.select("svg"),
            width = svg.attr("width"),
            height = svg.attr("height"),
            radius = Math.min(width,height)/2; // make full of the space of svg
        var g = svg.append("g")
            .attr("transform","translate("+width/2+","+height/2+")");
        var color = d3.scaleOrdinal(['#4daf4a','#377eb8','#ff7f00','#984ea3','#e41a1c']);
        var pie = d3.pie(); // generate the pie
        var arc = d3.arc() // generate the arc
            .innerRadius(0)
            .outerRadius(radius);
        var arcs = g.selectAll("arc")
            .data(data)
            .enter()
            .append("g")
            .attr(scaleOrdinal()"class","arc"); // generate the arcs group
        // draw the path
        arcs.append("path")
            .attr("fill",function(d,i){
                return color(i);
            })
            .attr("d",arcs);
    </script>
</body>olor(2));
        console.log(color(3));
        console.log(color(4));
        console.log(color(5));
    </script>
</body>
```

![1565484844941](/home/shi/.config/Typora/typora-user-images/1565484844941.png)

原始的color向量只有五个元素，而我们在代码最后一行中要求访问第六个元素。通过控制台可以看到，第六个元素值即为第一个元素值，这意味着第六个元素是对第一个元素的引用。由此可以看出，d3.scaleOrdinal()为我们创建了一个类似循环队列的数据结构。其元素满足：
$$
d(i)=d(i+k*d.length)
$$
使用d3.pie(),d3.arc()绘制饼图：

```html
<body>
    <svg width="300" height="200"></svg>
    <script>
        var data = [2,4,8,10];
        var svg = d3.select("svg"),
            width = svg.attr("width"),
            height = svg.attr("height"),
            radius = Math.min(width,height)/2; // make full of the space of svg
        var g = svg.append("g")
          .attr("transform","translate("+width/2+","+height/2+")");
        var color =
  d3.scaleOrdinal(['#4daf4a','#377eb8','#ff7f00','#984ea3','#e41a1c']);
        var pie = d3.pie(); // generate the pie
        var arc = d3.arc() // generate the arc
            .innerRadius(0)
            .outerRadius(radius);
        var arcs = g.selectAll("arc")
            .data(data)
            .enter()
            .append("g")
            .attr("class","arc"); // generate the arcs group
        // draw the path
        arcs.append("path")
            .attr("fill",function(d,i){
                return color(i);
            })
            .attr("d",arcs);
    </script>
</body>
```

