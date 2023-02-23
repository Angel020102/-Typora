## 你知道的flex

1. FLex是什么?

   - Flex 意为 弹性布局 任何一个容器都可以指定Flex布局
   - 设置Flex布局后 子元素的 float  clear  vertical-align 属性会失效

2. 基本概念

   - 采用Flex布局的元素 称为 Flex 容器 ,子元素会自动成为容器的 项目 (成员)
   - 容器默认有两根轴, 水平轴(主轴) 和垂直轴(交叉轴)

3. 容器的属性

   - 6 个属性设置在容器上 

     1.  **flex - direction 属性决定主轴的方向**

        - row (默认值) ：主轴为水平方向，起点在左端
        - row - reverse : 主轴为水平方向，起点在右端
        - column : 主轴为垂直方向，起点在上沿
        - column : 主轴为垂直方向，起点在下沿

     2. **flex - wrap 默认成员都排在一条线上 排不下改如何换行**

        - nowrap (默认) ：不换行
        - wrap ： 换行，第一行在上方
        - wrap - reverse : 换行，第一行在下方

     3. **flex - flow  是  flex - direction 属性 和 flex - wrap 属性间歇形式**

     4. **justify - content 属性定义而来项目(成员)在主轴的对齐方式**
- flex - start (默认值) ： 左对齐
        - flex - end ：右对齐
        - center ： 居中
        - space - between : 两端对齐，项目之间的间隔都相等
        - space - around ： 每个项目两侧的间隔相等
        
5. **align - items 属性定义项目在交叉轴上如何对齐**
     - stretch (默认值) ： 如果项目未设置高度或者auto，将占满整个容器
   - flex - start : 交叉轴的起点对齐
        - flex - end : 交叉轴的终点对齐
        - center ： 交叉轴的中点对齐
        - baseline ：项目的第一行文字的基线对齐
        
     6. **align - content  属性定义了多根轴线的对齐方式,如果项目只有一根轴线, 该属性不起作用**

        - stretch (默认值) ： 轴线占满整个交叉轴

        - flex - start : 与交叉轴的起点对齐
   - flex - end ：与交叉轴的终点对齐
        - center : 与交叉轴的中点对齐
        - space - between ： 与交叉轴两端对齐, 轴线之间的间隔平均分布
        - space - around : 每根轴线两侧的间隔都相等
        - space - around : 每根轴线两侧的间隔相等
   
4. 项目的属性

   - 6个属性设置在项目上
     1. order 属性定义了项目的排列顺序， 数值越小， 排列越靠前，默认为0
     2. flex - grow 属性定义了项目的放大比例，默认为0，即存在剩余空间，也不放大
     3. flex - shrink 属性定义了项目的缩小比例，默认为1，即空间不足，该项目缩小
     4. flex - basis 属性定义了在分配多余空间之前，项目占据的主轴空间， 浏览器根据这个属性， 计算主轴是否有多余的空间 ，它的默认值为auto，即项目的本来大小。 它可以跟width或beight属性一样的值 （350px）项目占据固定的空间
     5. flex 属性是 flex - grow  flex - shrink 和 flex - basis 的 简写 默认 （0,1，auto），后两可选，建议优先使用这个属性，而不是单独写三个分离的属性，因为浏览器会推算相关值
     6. align - self 属性允许单个项目有其他项目不一样的对齐方式，可覆盖 align - items 属性， 默认为auto ，表示继承父元素的align - items属性， 如果没有父元素目，则等同与 stretch

### 阮一峰的骰子布局

- https://www.ruanyifeng.com/blog/2015/07/flex-examples.html

