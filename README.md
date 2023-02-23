# HTML

## HTML5

- 语义标签
- 去除 HTML4 不常用的标签

### 增强表单标签

- input 标签
  - type 属性
    - url
    - email
    - number(0-9,e) e是科学计数法
      - max 属性
      - min 属性
    - range 滑动器
    - search 搜索框（右边有x按钮）
    - date
    - week
    - month
    - color
    - password
      - maxlength
      - minlength
    - ** 更多严格判断使用 JS 校验**

  - placeholder
  - autofocus 自动获取焦点

### 增强结构标签

- header
- nav
- article 文章
  - section 章节
- aside
- footer


### 音视频标签

- audio
  - src
  - controls
  - 开始标签和结束标签之间提示不支持标签的浏览器
  - source 支持多个音频格式
    - src
      - **.mp3**
      - .ogg
  - `<embed src="1.mp3" />`

- video
  - src
  - source
    - src
    - type
      - **video/mp4**
      - video/ogg
      - video/webm
  - `<object data=1.mp4 width="320" height="240">`
    - `<embed src="1.swf" width="320" height="240" />` 引入音频或视频
      - src
        - mp3
        - mp4

### 其他语义标签

- figure(类似dl和dt/dd)
  - figcaption

- details 展示文章的细节
  - summary
  - p 
    - mark 着重标签
  - p

- meter 刻度标签
  - max 最大值
  - min 最小值
  - value 当前值
  - low 自己定义的最小值
  - hight 自己定义的最大值

- progress 进度条
  - max
  - value

```
<input type="text" list="city">
<datalist id="city">
  <option value="IBM">IBM</option>
  <option value="xiaomi">xiaomi</option>
</datalist>
```


- canvas 画布标签
