# html

## 语义标签

html的标签虽自带基础样式（比如h1标签字体会加粗放大），但样式主要还是通过css设置；虽然基本不使用标签的默认样式，但还是应该根据内容选择对应的语义标签，这样可以增加代码的可读性，也好使用标签选择器，选择对应语义的元素集中设置样式；如果只是将元素组合在一起，又找不到合适的语义化标签，可以使用div和span，它们应该配合class使用，以便轻松定位

## a标签

1. 支持title属性，鼠标放上去会显示title属性内容（很多元素都支持title属性）

2. 可用于定位，比如href="#id1"，点击页面会滑动到id1所在位置

3. 默认打开index.html文件，比如href="[https://www.example.com/project/]()"，浏览器会打开project目录下的index.html

4. download属性，单独添加download属性，指示下载href链接对应的文件；也可以download="filename"，下载的文件将会被重命名为filename

5. target="_blank",实现在新标签中打开页面（默认会替换当前标签页地址）

## img标签

1. src指定图像地址，alt添加图像描述，该描述在图像加载失败时会展示，另外img也支持title属性

2. 给图像添加width和height，除了指定图像大小，还可以在图像未完成加载时留在对应大小的空白，图片加载完成时不会导致html元素位置发生移动

3. 默认情况下，img会将指定的宽度和高度填满，图片的原始尺寸不一定和设置的宽高比一致，这样就可能会导致图片比例失真，解决方法是只指定高度或宽度，另一项不设置或者设置为auto，使图片锁定一边的尺寸，另一边按照比例自动扩展；如果就要限定宽高，可以使用object-fit属性，该属性配置为cover时，图片保持比例填满容器（可能被裁剪），配置为contain时，保持比例完整显示（可能有留白）

4. img作为背景图片时会有些不同，首先它不会根据宽高填满容器，而是保持原图片大小，只显示一部分或者留下空白（如果不设置no-repeat，会重复图片填充空白部分），它也支持cover和contain模式，不过是用background-size属性指定

## input标签

1. 为input增加label，除了作为标签，点击label区域会触发input，可以增加input可触发面积，这对于移动端通常很有用；将label和input关联起来有两种办法，一是是将input置于label内部，二是将label的for属性与input标签的id属性关联，第一种方式只能将标签置于input前面，灵活性较差

2. input默认字体属性是不继承上级元素的，需要单独指定font-size:inherit来使input字体与页面字体保持一致

3. 功能方面：input支持多种type，比如date，time，datetime来选择日期，时间和日期加时间；file支持文件上传（增加mutiple属性支持上传多个文件，通过input的dom对象可读取上传文件列表，但文件信息只包含name，size和type，浏览器出于安全考虑，不提供文件路径的访问）；color是调色器，可选择颜色；还可以贴图片，返回点击区域的坐标

4. 数据校验方面，支持邮箱，数字，最大最小值，是否必输等检查，还可通过pattern属性增加正则表达式校验，配合伪类选择器（主要是input:required,input:invalid）可轻松选择限制了必输但没输入内容，或者输入内容与格式要求不一致的单元格

5. appearance:none取消表单的默认样式(不同浏览器上表单自带的样式可能不一致)，通过自行设定样式，来实现页面在各版本浏览器上的ui一致

6. datalist可配合input使用，为input提供输入建议，方式是将input的list属性与datalist的id属性相关联

# 页面布局

## float

float原本用于设置文字环绕图片，后面也被用于页面布局，在css增加了flexbox和grid支持后，已经不再需要通过float布局了，现在应只在文字环绕图片场景使用float

## flexbox

用于一维布局，使元素横向或者纵向排列（虽然是一维，但也可设置在内容超限后自动换行，所以也不是只能用于一行或一列的布局）；flex包含flex container和flex items，需要先将父元素设置为flex container（通过display:flex），将父元素设定为flexbox容器后，子元素会按照flexbox排列

### 容器属性

1. flex-direction:row|column，指示项目按行或者按列排列，默认按行

2. flex-wrap，换行控制，默认为nowrap即不换行，设置wrap在项目超出时换行

3. justify-content，主轴对齐（按行排列时就是水平居中，按列排列时就是垂直居中）；常用的是center居中对齐，另外还有space-between（项目间等距，两端无边距），space-around（项目两侧等距）

4. align-items，交叉轴（与主轴垂直，主轴水平时交叉轴就是纵轴）对齐

### 项目属性

1. order指示排列顺序

2. flex-grow指示扩展比例，在容器有剩余空间时，按照比例扩展，默认0不扩展（分配的是剩余空间）

3. flex-shrink指示收缩比例，在容器空间不足时，控制是否允许收缩，默认1允许收缩（压缩的是超出的空间）

4. flex-basic基础大小，按照基础大小分配完空间后，根据容器空间使用情况，再施加flex-grow或flex-shrink影响（容器空间有剩余就使用flex-grow扩展，超过容器空间了就使用flex-shrink收缩，只有其中一个会发挥作用）

5. 简写属性为flex，按顺序分别指定flex-grow，flex-shrink和flex-basis

## grid

用于二维布局，将容器划分为二维网格，可以配置网格的行列数，间距，大小，并且可以指定子元素占据哪些网格（可以指定占据多个行列）；默认是一个单列的网格，子元素自上而下排列在网格中

1. display:grid开启网格布局，显式网格设置网格的行数和列数，gap设置网格间距（也可以分别指定行间距和列间距），justify-items和align-items设置网格项在它们各自单元格内的对齐方式（当网格项尺寸小于单元格时），justify-content和align-content设置整个网格在容器内容的对齐方式(当grid小于容器时)；注意在grid容器上设定的这四个对齐属性，都不是网格内容在网格中的对齐方式

2. 显式网格和隐式网格。显式网格：通过grid-template-rows和grid-template-columns明确定义网格的行数和列数；隐式网格：通过grid-auto-rows和grid-auto-columns设置在项目数超过显式网格定义的数量时，自动生成网格的宽度和高度；

3. 行列尺寸可以使用固定值，也可以使用fr（会按照比例分配，和flexbox中的flex-grow直接设置数字一样）；设置多行或多列时可以使用repeat，比如repeat(4,1fr)就是设置4格1fr的项；使用minmax来设置最小和最大尺寸；repeat(auto-fit,minmax(230px,1fr))不指定具体列数，而是让grid自动在一行尽量多的放置230px宽度的列

# 其他

## position

1. 默认值为static即静态定位，元素位于正常文档流，top/bottom/left/right/z-index属性失效

2. releative相对定位，元素相对于自己在正常文档流中的定位，可通过位置属性设置基于原位置的偏移，不加位置属性就还在处于原位置（跟没加releative定位一个样），常用于作为absolute定位元素的上下文

3. absolute绝对定位，脱离文档流，下方元素会上浮占据它的位置，它是基于最近的非static的祖先元素定位的，如果没有这样的祖先，就基于视口定位；绝对定位元素默认会处于单独的层（z-index大于默认文档流，会覆盖默认文档流内容），对绝对定位元素的布局设置不会影响正常文档流，所以它常用来做dialog弹框功能

4. fixed，固定定位，基于视口定位，用来做固定表头或者页面助手的功能

5. sticky，粘性定位，它必须设置一个阈值，达到阈值前正常定位，达到阈值后固定（通常是top等于多少时固定住）；要注意它并不是默认基于body的，而是基于它最近可滚动的祖先，只是说body默认是overflow:auto可滚动的，所以sticky元素到body之间如果没有可滚动元素就会基于body；如果目标就是要设置基于body粘性，要检查下sticky到body之间还有没有其他可滚动的元素

## css选择器

1. 基本：*通用选择器，div元素选择器，.class类选择器，#id id选择器

2. 属性选择器：[disabled]存在该属性，[type="text"]精确匹配属性值；[class*="btn"]属性包含指定字符串；[href^="https"]属性值以指定字符串开头；[src$=".png"]属性值以指定字符串结尾；

3. 组合选择器：nav ul后代选择器（所有后代，不限层级）；ul > li 仅选择直接后代；h2 + p 相邻兄弟选择器（仅选择紧邻的下一个元素）；h2 ~ p 通用兄弟选择器，选择后面所有同级元素

4. 伪类选择器：
   
   1. 动态伪类：a:link,a:visited,a:hover,a:active根据链接状态选择
   
   2. 结构伪类：第一最后一个子元素，指定第几个子元素，奇数行偶数行（:first-child,:last-child,nth-child(n),nth-of-type(odd|even)）
   
   3. 表单伪类：input:checked,input:disabled,input:required,input:valid,input:invalid选择选中的表单，禁用的表单，设置了必填的表单，检查结果为有效和无效的表单（这个配合pattern正则表达式检查，功能强大，可增加伪元素表示当前输入是否通过检查）
   
   4. 伪元素选择器：::first-line,::firt-letter,::before,::after选择文本的第一行，第一个字母，签收插入内容（::before{content:":cat:"} 插入个猫图标，还可以设置字体，边距等css属性）
