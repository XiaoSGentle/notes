	* px ，，百分比*
	* vw:相对于视口宽度的 1%
	* vh:相对于视口高度的 1%
	* em:相对于元素的字体大小（font-size）（2em 表示当前字体大小的 2 倍）
	* rem:相对于根元素的字体大小（font-size）
* 大小`width，height`
	* max-width,height-最大宽度、高度
	* min-width,height-最小宽度、高度
* 内边距 `padding`
* 外边距 `margin`
	* auto ：块级元素可以==水平居中==，如果未设置 `width` 属性（或将其设置为 100％），则居中对齐无效。
	* 图片居中：  margin-left: auto;  margin-right: auto;
	* 文字垂直居中：line-height：height；
* 文本对齐 `text-align`
	* center ==文本居中对齐==
	* left
	* right
	* justfiy
* 文本方向 `direction`
	* rtl 右边开始
* 文本装饰  `text-decoration`
	* overline 上滑线
	* line-through
	* underline
* 文字间距 `text-indent`
* 边框：`border-right` 属性添加到 ==li==标签上以创建链接分隔符
	*  图片边框：border-image: url(border.png) 30 round;
* 字体 `font-family`
* 圆角`border-radius`
* 连接状态
	*   `a:link` - 正常的，未访问的链接
	-   `a:visited` - 用户访问过的链接
	-   `a:hover` - 用户将鼠标悬停在链接上时
	-   `a:active` - 链接被点击时
	- ![[Vue.enclosure/Pasted image 20221028163942.png]]
- 块级元素
	- 占一行的：div,h1-h6,p,form,header,footer,section
	- 仅占所需元素：span，a，img
* 隐藏显示元素 
	* display
		* ==inline-block== 用于水平而不是垂直地显示列表项
		* ==inline：行排列==
		* `display: block;` - 将链接显示为块元素可以使整个链接区域都可以被单击（而不仅仅是文本）可以设置大小以及边距
		* none:隐藏不占位置
	* visibility
		*  hidden：隐藏占位置
* 定位 `position`
	*  static：静态定位的元素**不受** top、bottom、left 和 right 属性的影响。
	-   `relative`（父容器）：设置相对定位的元素的 top、right、bottom 和 left 属性将导致其偏离其正常位置进行调整。
	-   fixed：**相对于视口定位的**，这意味着即使滚动页面，它也始终位于**同一位置**。常用做导航！
	-   `absolute`（子容器）: 相对于**最近的**定位**上层元素**进行定位
	-   sticky
- 溢出 `overflow`
	*   ==overflow: auto==;若有溢出
	-   visible - 默认。溢出没有被剪裁。内容在元素框外渲染
	-   scroll - 溢出被剪裁，同时添加滚动条以查看其余内容
	-   `hidden` - 溢出被剪裁，其余内容将不可见
	-   auto - 与 scroll 类似，但仅在必要时添加滚动条
- 浮动`float`
	-   left - 元素浮动到其容器的左侧		
	-   right - 元素浮动在其容器的右侧
	-   none - 元素不会浮动（将显示在文本中刚出现的位置）。默认值。
	-   inherit - 元素继承其父级的 float 值
- 清除浮动 `clear`
	-   none - 允许两侧都有浮动元素。默认值
	-   left - 左侧不允许浮动元素
	-   right- 右侧不允许浮动元素
	-   both - 左侧或右侧均不允许浮动元素
	-   inherit - 元素继承其父级的 clear 值
- 透明度`opacity`
	- 经常与hover一起使用
	- RGBA透明度:==background: rgba(76, 175, 80, 0.3)== /* Green background with 30% opacity */
* 导航问题
	* `list-style-type: none;` - 删除项目符号。导航条不需要列表项标记。
	* 使用position: fixed;    top: 0;    width: 100%; 将导航固定上下。
	* 为 ul 添加 `position: sticky;`，以创建粘性导航栏。

* 背景
	* `background-size`contain（可能会有暴露区域），cover（无暴露区域）
* 阴影
	* text-shadow
	* box-shadow
	* text-shadow: 1px(水平阴影) 1px(垂直阴影) 2px(阴影大小) black(阴影颜色), ==可以设置多种颜色格式如前==;
* 2D转换
	* 网页链接https://www.w3school.com.cn/css/css3_2dtransforms.asp