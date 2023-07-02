# Hello D3

## - 使用D3获取、修改、删除节点（图元）

### 	- D3的基础

## - 比例尺

### 	- 线性比例尺

### 	- “条带”比例尺

## - 实例：使用D3绘制简单柱状图

## - 引入坐标轴

### 	- 配置坐标轴根据剩余时间



## 元素的标识

- 操作元素首先需要知道元素的标识
  - 即要得到已有或已经创建的元素

- 元素的ID
  - 可以唯一找到元素的标识符

- 元素的Class
  - 人为赋予的“类别”可以标记元素的集合，其中的元素标签可以不相同

- 元素的标签
  - HTML自带的标签名称，可以找到一批同类别的物体，如所有的“矩形”
  - 使用自带的标签往往难以直接索引到目标元素
  - <rect>,<circle>,<svg>等

## 使用D3查询SVG

- d3.select(...)
  - d3.select('#rect1')
  - 查询ID为'rect1'的元素
  - #表示后面的字符串是一个ID

- d3.selectAll(...)
  - d3.selectAll('.class1')
  - 查询所有class是'class1'的元素
  - d3.selectAll('rect')
  - 查询所有标签是'rect'的元素（rect为SVG中的矩形标签）

- 基于层级的查询：
  - d3.select('#maingroup rect')
  - d3.selectAll('.tick text')
  - d3.selectAll('#secondgroup rect')

- 如：'#secondgroup rect'
  - 首先会找到id为secondgroup的标签
  - 进一步找到secondgroup的子标签中的rect
  - 仍然是对rect做查询，但只是结果通过父标签做了筛选
  - 注意：这种形式的查询，经常会在配置坐标轴的代码中使用，请至少熟悉其形式！

- d3.select(...)也可用于查询类别，如
  - d3.select('.class1')
  - 但只会返回找到的第一个元素

- 因此对于class、标签名称的查询建议使用d3.selectAll
- 对于特定某一个元素的查询建议使用d3.select

## 使用D3设置SVG中的属性

- 常见的属性
  - id.class（特殊的属性，可以用.attr设置）
  - x，y，cx，cy（注意屏幕的坐标系）
  - fill，stroke
  - height，width，r（圆的半径）
  - transform ->translate，rotate，scale

- SVG的属性非常多，且属性的取值范围&类型各不同
  - tip1：尽可能记住一些常见的属性，以提高编程速度
  - tip2：遇到不认识or想要设置某个属性，一定要学会查阅
  - <rect id='rect3' class='class1'
  - stroke='black' height='200' width='66' fill='#7289AB' x='300' y='-100'>

- 屏幕空间的坐标系于常见坐标系不同
  - 左上方为原点
  - y，x分别垂直向下，水平向右

## element.attr(...)

- 设置元素的属性：element.attr('attr_name','attr_value')
  - rect1.attr('y','100')
  - d3.select('#rect1').attr('y','100')

- 获取元素的属性：element.attr('attr_name')
- 注意：数值类型的数据扔在HTML中仍由字符串存储
  - 要使用+(strValue)及时转换，如let value = +('233.666');
  - 活用模板字符串，如
    - let width = 666;
    - attr('transform',` translate(0,${width+100}) `)
    - 模板字符串用``表示（本质上还是一个字符串）
    - ${...}可以在字符串中嵌入程序表达式

- DOM
  - 父节点的属性会影响子节点
  - 子节点的属性会相对于父节点

- 活用<g>节点可以省点很多冗余代码！！
  - d3.select('#maingroup')
  - .attr('transform','translate(200,100)')

## 使用D3添加和删除元素

- element.append()
  - const myRect = svg.append('rect');
  - const myRect = d3.select('#mainsvg').append('rect')
  - const myRect = d3.select('#mainsvg').append('rect').attr('x','100')

- 链式添加
  - const myRect = d3.select('#mainsvg').append('g').attr('id','maingroup')
  - .append('rect').attr('fill','yellow')

- 链式调用：习惯这种编程习惯有助于大家使用D3和浏览D3的代码
- element.remove()
  - 请小心使用
  - 会移除整个标签

- 在debug的过程中可以考虑使用'opacity'属性hack出移除的效果
  - element.attr('opacity','0')

## 数据的读取-CSV数据

- d3.csv(...)
  - 读取目标路径下的某一个csv文件。
  - e.g.,d3.csv('static/data/hello.csv');

- d3.csv是一个JavaScript异步函数：
  - 不可以直接获取它的返回值，如；
  - let myData = d3.csv('static/data/hello.csv');

- d3.csv('path/to/data.csv').then(data=>{//‘数据读取后的代码逻辑’})
  - 要通过.then(data=>{...})的方式来获得读取后的数据
  - then(...)中的‘data=>{...}’是一个函数。
  - 此函数接受的输入（参数），即data，为读取后的数据。

## D3.JS的数值计算

- 数据可视化常常涉及对数据的处理与计算：
  - 下述三个接口分别用于计算数组的最大值、最小值、[最小值，最大值]。

- d3.max(array)
  - 返回数组中的最大值。
- d3.min(array)
  - 返回数组中的最小值。
- d3.extent(array)
  - 同时返回最小值与最大值，以数组的形式，即[最小值，最大值]。

- 数组中的内容可以是任意对象：
  - 每个对象可能包含多个属性。
  - 具体取哪个属性的最大值通过回调函数来提示d3.max，d3.min与d3.extent。

- e.g
  - let a = [
    {name: 'Shao-Kui', age:20, height: 176}, {name:'Wen-Yang', age:24, height: 
    180}, {name:'Liang Yuan', age: 29, height: 172}, {name:‘Tam Hou', age:23, 
    height: 173}]
  - d3.max(a, d => d.age) // 29
  - d3.max(a, d => d.height) // 180
  - d3.extent(a, d => d.height) // [172, 180]
  - d3.min(a, d => d.age) // 20





## 比例尺

- 比例尺用于把实际的数据空间映射到屏幕的空间
- 比例尺非常重要，会经常同时传给坐标轴与数据！！

## Scale-Linear

const xScale = d3.scaleLinear().

.domain([min_d,max_d]).

.range([min,max]);



const xScale = d3.scaleLinear()

.domain([0,d3.max(data,datum=>datum.value)])

.range([0,innerWidth]);



d3.max(数据，回调：如何提取数据的值)

d3.max：求出数据某一属性的最大值，比如年龄的最大值

![image-20230701142618530](C:\Users\86136\AppData\Roaming\Typora\typora-user-images\image-20230701142618530.png)

![image-20230701142640076](C:\Users\86136\AppData\Roaming\Typora\typora-user-images\image-20230701142640076.png)

## Scale-Band

const yScale = d3.scaleBand()

.domain(list)

.range([min,max])

.padding(p);



const yScale = d3.scaleBand()\

.domain(data.map(datum=>datum.name))

.range([0,innerHeight])

.padding(0.1);

<img src="C:\Users\86136\AppData\Roaming\Typora\typora-user-images\image-20230701142654010.png" alt="image-20230701142654010" style="zoom:0%;" />

## Scale是函数

- 通过d3.scaleLinear或d3.scaleBand得到的返回值本质上是函数
  - 给出数据中的值（domain）
  -  返回映射后的值（range）
  -  .domain(...)和.range(...)可以理解为配置这个函数（功能）的过程

- 比例尺的 定义仍适用链式调用

![image-20230701143111437](C:\Users\86136\AppData\Roaming\Typora\typora-user-images\image-20230701143111437.png)









## BarChart的实现

- 使用select，selectAll，append，attr来实现柱状图

  - 数据暂时通过前端给定（Fake Data,下节课正式进入Data-join）

    ```html
    <!DOCTYPE html>
    <html>
      <head>
        <title>BarChart! </title>
        <script src="../d3/d3.min.js"></script>
      </head>
      <body>
        <svg width="1600" height="800" id="mainsvg" class="svgs"></svg>
        <script>
            const data = [{name: 'Shao-Kui', value:6},
            {name:'Wen-Yang', value:6}, {name:'Cai Yun', value:16}, {name:'Liang Yuan', value: 10}, 
            {name:'Yuan-Chen', value:6}, {name:'Rui-Long', value:10}, {name:'Dong Xin', value:12}, 
            {name:'He Yu', value:20}, {name:'Xiang-Li', value:12}, {name:'Godness', value:20}, 
            {name:'Wei-Yu', value:15}, {name:'Chen Zheng', value:14}, 
            {name:'Yu Peng', value:15}, {name:'Li Jian', value:18}]; 
    
            const svg = d3.select('#mainsvg');
            const width = +svg.attr('width');
            const height = +svg.attr('height');
            const margin = {top: 60, right: 30, bottom: 60, left: 150};
            const innerWidth = width - margin.left - margin.right;
            const innerHeight = height - margin.top - margin.bottom;
    
            const g = svg.append('g').attr('id', 'maingroup')
            .attr('transform', `translate(${margin.left}, ${margin.top})`);
    
            const xScale = d3.scaleLinear().
            domain([0, d3.max(data, datum=>datum.value)]).
            range([0, innerWidth]); 
    
            const yScale = d3.scaleBand().
            domain(data.map(datum => datum.name)).
            range([0, innerHeight])
            .padding(0.1);
    
            data.forEach(datum => {
              g.append('rect')
              .attr('width', xScale(datum.value))
              .attr('height', yScale.bandwidth())
              .attr('y', yScale(datum.name))
              .attr('fill', 'green')
              .attr('opacity', '0.8')
            })
    
            const yAxis = d3.axisLeft(yScale);
            const xAxis = d3.axisBottom(xScale);
            g.append('g').call(yAxis);
            g.append('g').call(xAxis).attr('transform', `translate(0, ${innerHeight})`);
    
            
            d3.selectAll('.tick text').attr('font-size', '2em');
    
            g.append('text').text('Members of CSCG').attr('font-size', '3em')
            .attr('x', innerWidth / 2 - 200).attr('y', -10)
        </script>
      </body>
    </html>
    ```

## 定义Margin

- SVG对于D3.js是一个画布
- SVG范围外的任何内容属于画布之外，浏览器将不予显示
- 定义margin
  - const margin = {top:60,right:30,bottom:60,left:200}

- 计算实际操作的inner长/宽
  - const innerWidth = width - margin.left - margin.right;
  - const innerHeight = height -margin.top - margin.bottom;

- 在SVG下额外定义一个组作为新的根节点
  - const g = svg.append('g').attr('id','maingroup')
  - .attr('transform',`translate(${margin.left},${margin.top})`);

![image-20230701150954545](C:\Users\86136\AppData\Roaming\Typora\typora-user-images\image-20230701150954545.png)

## 引入坐标轴

- 一个坐标轴为一个group(<g>)，通常我们需要两个坐标轴
- 坐标轴中包含：
  - 一个<path>用于横跨坐标轴的覆盖范围
  - 若干个刻度
    - 每个刻度也是一个group
  - 每个刻度下属还会包含一个<line>和一个<text>
    - <line>用于展示坐标轴的轴线，如左到右或上到下
    - <text>用于展示坐标轴的刻度值，如实数、姓名、日期
  - 一个标签用以描述坐标轴

- 定义坐标轴（获得结果是函数）：
  - const yAxis = d3.axisLeft(yScale);
  -  const xAxis = d3.axisBottom(xScale);
  -  axisLeft: 左侧坐标轴
  -  axisBotton: 右 侧坐标轴

- 实际配置坐标轴：
  - const yAxisGroup = g.append('g').call(yAxis);
  -  const xAxisGroup = g.append('g').call(xAxis)

- 实际配置后会发现<g>中增添了与坐标轴相关的元素
- 任何坐标轴在初始化之后会默认放置在坐标原点，需要进一步的平移

## 配置坐标轴

- 可以对坐标轴的风格进行修改：d3.selectAll('.tick text').attr('font-
  size', '2em’);

- .tickSize来设置网格线

- 坐标轴的标签加入不在D3接口的负责范围内：

  - 通过对应组.append(‘text’)来人为实现

  - （左）纵轴坐标需要.attr( ‘transform’, ‘rotate(-90)’) 来旋转

  -  纵轴坐标旋转后，x / y 会颠倒甚至取值范围相反

  -  回忆DOM：父节点的属性会影响子节点，而坐标轴默认的’fill’属性是

    ‘none’，因此请一定手动设置文字颜色.attr('fill', 'black')









# Data-Join

## What is Data-Join

- 本质上是将数据与图元进行绑定
  - 每个国家的人数绑定到矩形的长度
  - 疫情感染的人数比例绑定到圆的半径

- Why
  
  - 以数据为中心（Data-Driven）的可视化操作：
    - 根据数据自动调整图元的属性
    - .attr(...)接口可基于图元自己绑定的数据自动调整属性值。
  
  - 数据发生变化时可以自动对图元增删改查：
    - 不再需要手动添加、修改、删除图元。
    - 根据数据的增加or删除or更新，自动补充or移除or更新图元。

- Data-Join并不是必要的操作，不使用Data-join同样可以画出所有可视化作品。
- Data-Join只是让D3.js编程变得更加高效且语法更简洁。

- d3.selectAll(‘.class’).data( dataArray )
- dataArray在保证是一个数组的前提下可以是任何形式：
  - e.g., [0, 2, 32, 18];
  - e.g., [{name: ‘Song-Hai’, value:42}, {name:’ Yuan’, value:30}, 
       {name:‘Wen-Yang', value:16}, {name:‘Shao-Kui', value:20}];
- 单独用.data(...)只可针对数据和图元数目相同的情况：
  - dataArray是一个数组，其中的每‘条’数据会与一个图元绑定。
- 默认的绑定按照双方的索引顺序：
  - （Data的Key：后续D3中会讨论。）
- 不调用.data(...)，则图元不会与任何数据绑定！
- 数据的更新只需要重新绑定另一个dataArray 即可。

![image-20230702114039206](C:\Users\86136\AppData\Roaming\Typora\typora-user-images\image-20230702114039206.png)

- 调用形式：
  - d3.selectAll(‘.class’).data(myData).join(‘
    图元’).attr(d => ...).attr((d, i) => ...)
  - .join(...)会根据数据的条目补全or删除图元。

- 若有新增的数据，则会自动增加对应图元。

- 若有修改的数据，则会自动更新对应图元。

- 若有删除的数据，则会自动移除对应图元。

![image-20230702114444755](C:\Users\86136\AppData\Roaming\Typora\typora-user-images\image-20230702114444755.png)

## 用函数设置图元属性

- selection.attr(‘attrbuteName’, ‘value’)
  - 通过值设置属性。
- selection.attr(‘attrbuteName’, (d, i) => {...})
  - 通过函数设置属性，函数的输入为绑定的数据，返回值为图元得到的属性值。
  - d为Data-Join中，‘.data(array)’绑定给每个图元的数据。
  - i为Data-Join中，‘.data(array)’绑定图元的顺序，即图元对应原本数组的
    第几个。
  - e.g., d3.selectAll(‘rect’).attr(‘width’, (d, i) => 1000 * d.age ); 
  - e.g., d3.selectAll(‘circle’).attr(‘cy’, (d, i) => 200 * i + 30); 
  - 由于绑定数据的不同，故得到的结果也不同。
- 设置图元属性的函数遵循如下规则（顺序性）：
  - 函数可仅使用d => {...}，即只有一个参数，但此时函数体无法使用索引。
  - 即使未使用到绑定的数据，如需使用索引，仍需要完整的写出(d, i) => {...}。







## 动画

- 动画D3.js的Transition模块：
  - d3.transition与时长控制
  - 动画的过度（Ease）
  - 动画的同步
  - 动画的继承
  - 动画的插值
  - 动画的循环
- 基于动画绘制动态散点图
  - 数据的清洗与预处理
- 基于动画绘制追逐柱状图
- Tip：数字的格式化
- Tip：图元的起始状态
