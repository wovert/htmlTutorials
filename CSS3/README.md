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
6. 设置父元素(.clearfix)样式：最佳解决方案
	+ `.clearfix:after {content:'';clear:both;display:block}`
	+ IE6 下： `.clearfix { *zoom:1;}`
		* zoom 缩放 : 触发 IE下 haslayout，使元素根据自身内容计算宽高。FF 不支持。
7. overflow:hidden 清除浮动；
	+ 问题：需要配合宽度或者 zomm 兼容 IE6/&

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

# position
- IE6/7: filter: alpha(opacity=50) // 0-100
- other: opacity: .5; // 0-1 (0完全透明)
- IE6 不支持 fixed 固定定位


# 兼容性
1. H5 标签兼容
- IE6/7 不兼容
- [html5shiv.js](https://github.com/aFarkas/html5shiv) 库解决

2. IE6: 元素浮动之后，能设置宽度的话就给元素加宽度。如果需要宽度是内容撑开，就给它里边的块元素加上浮动。
- solution：.innerTag {float:right|left}

3. IE6: 第一块元素浮动，第二块元素加 margin 值等于第一块元素。在 IE 6 下会有间隙问题。
- solution：浮动解决间隙问题

4. IE6: 子元素超出父元素宽高，会把父级的宽高撑开
- solution: 不要子元素宽高高出父级宽高

5. p 包含块元素嵌套规则
- solution: p/hn/td 元素只能包含内联元素和内容，不能包含块元素

6. margin 兼容性问题
- 问题：margin-top 传递。父元素没有margin-top，子元素设置 margin-top 会影响到父元素margin-top。
	+ solution：触发 BFC, 给父元素加边框或 overflow:hidden（border:1px），但 IE6 不能解决
	+ solution: 触发 haslayout, zoom:1; 在 IE6解决 

- 问题：兄弟元素上下 margin会叠加，不会累加。以元素最大的值 margin 显示
	+ solution: 尽量使用同一方向的 margin。比如都设置 margin-top 或者 margin-bottom。

7. display: inline-block; IE6下 垂直显示问题
- 问题：IE6 不支持inline-block
- 解决：*display: inline; *zoom:1;

8. IE6 最小高度问题
- 问题：IE6 下height: 1px; 默认19px最小高度显示
- 解决：overflow: hidden;

9. IE6 双边距
- 问题：当元素**左浮动后**，在设置 margin-left 就会产生双倍边距
- 解决(IE6/7)：*display: inline;

10. li 里元素都浮动 li 在 IE6/7 下方会产生 4px 的间隙问题
- 解决(IE6/7)：li {*vertical-align:top}

11. 浮动元素之间注释，导致多复制一个文字问题
- 解决：