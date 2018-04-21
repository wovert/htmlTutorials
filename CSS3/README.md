# Block element and inline element
- Block Element
	+ default single a line
	+ When no width, default streach a line
	+ all of CSS
- Inline Element
	+ 内排可以继续跟同类的标签
	+ 内容撑开宽度
	+ 不支持宽高
	+ 不支持上下文的 margin
	+ 代码换行被解析

- Inline-block (用于分页)
	+ 块在一行显示
	+ 内联支持宽高
	+ 默认内容撑开宽度
	+ 标签之间的换行间隙被解析（问题）
	+ ie6/7 不支持款属性标签的inline-block (问题)

# float/文档流
> 元素脱离文档流，按照指定方向发生移动，遇到**父级边界**或者**相邻的浮动元素**停了下来
- float: left|right|left|noe|inherit;

- clear: left|right|both|none|inherit; 元素的左边或右边不能有浮动元素
- clear: both; 在左右两侧均不允许浮动元素

# after 伪类
- div:after{content:"这是有after伪类生成的文字"}

# 清除浮动的方法
1. 加高度（扩展性不好）
2. 給父级浮动（页面中所有元素都加浮动，margin 左右自动失效, floats bad!）
3. .inline-block 请浮动方法(margin 左右 auto 失效)
4. 空标签清除浮动(IE6 最小高度19px; 解决后 IE6下还有2px 偏差)
5. .br 清浮动 （不符合工作中：结构/样式/行为，三者分离的要求）
6. 设置父元素样式：最佳解决方案
	+ `.clearfix:after {content:'';clear:both;display:block}`
	+ IE6 下： `.clearfix { *zoom:1;}`
		* zoom 缩放 : 触发 IE下 haslayout，使元素根据自身内容计算宽高。FF 不支持。



7. overflow:hidden 清除浮动；
问题：需要配合宽度或者 zomm 兼容 IE6/&

# BFC- block formatting context 标准浏览器
1. float 的值不为 none
2. overflow 的值不为 visible
3. display 的值为 table-cell, table-caption, inline-block 中的任何一个
4. position 的值不为 relative 和 static
5. width|height|min-width|min-height:(!auto)

# hashlayout IE 浏览器
- writing-mode: tb-rl
- -ms-writing-mode: tb-rl
- zoom:(!normal) 放大
	+ ！normal 指的是值为不正常时触发 hashlayout



