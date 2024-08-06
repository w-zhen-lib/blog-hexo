---
title: Hexo日历插件
date: 2024-08-06 09:34:06
tags: blog
categories: 知识库
top_img: https://picsum.photos/id/119/1920/600
cover: https://picsum.photos/id/119/800/600
---

# 日历插件
**博客搭建完成后的基本效果还行，但是每次按时间查看文章的归档记录都要去archive，并且一条直线也不是很直观，🙂所以准备引入一个日历插件**

## 环境
```shell
C:\Users\MrZhen>node -v
v20.16.0

C:\Users\MrZhen>npm -v
10.8.2

C:\Users\MrZhen>hexo version
hexo-cli: 4.3.2
```

## 风格
> 参考[添加文章日历插件](https://ouoholly.github.io/post/butterfly-add-hexo-generator-calendar/)

## 步骤
### 安装插件  
注意，网上的插件版本太多，我个人就是在npm拉取默认的插件后无法使用，后来去github搜索才找到了合适的插件，我当前的butterfly版本是**4.1.3**
参考的仓库地址：[hexo-generator-calendar](https://github.com/howiefh/hexo-generator-calendar)
```shell
npm install --save git://github.com/howiefh/hexo-generator-calendar.git
```

### js脚本引入  
引入两个js文件（js文件的保存目录和引入配置文件我都是放在了hexo层，没有修改主题层的代码，测试后也没有问题）  
- [calendar.js](/js/calendar.js)
- [language.js](/js/language.js) 
这一步引入方式不同，可以参考[butterfly官方]()，查看对应版本的js文件引入方式，我这个版本的引入方式如下
```yaml
# Inject
# Insert the code to head (before '</head>' tag) and the bottom (before '</body>' tag)
# 插入代码到头部 </head> 之前 和 底部 </body> 之前
inject:
  head:
    # - <link rel="stylesheet" href="/xxx.css">
  bottom:
    # - <script src="xxxx"></script>
    - <script src="https://code.jquery.com/jquery-3.7.1.min.js" integrity="sha256-/JqT3SQfawRcv/BIHPThkBvs0OEvtFFmqPF/lYI/Cxo=" crossorigin="anonymous"></script>
    - <script src="/js/calendar.js"></script>
    - <script src="/js/language.js"></script>
```
**大多数文章都是引入这两个文件，但我引入calendar.js就是一直报错未定义jQuery对象，可能我的网站刚搭建没多久，插件比较少，所以人家已经引入的js我没有😂，因此多引入了个js文件**

### 修改配置项
#### 组件名称
这一步修改butterfly的配置，找到网站语言对应的配置文件，我的是**themes\butterfly\languages\zh-CN.yml**
```yaml
aside:
  card_calendar: 文章日历
```
#### 组件开关
这一步再次回到hexo的全局配置文件中，增加开关
```yaml
aside:
  enable: true
  card_calendar: #位置不是固定的，按自己的需求来 
    enable: true #日历组件
```

### 添加代码
- themes\butterfly\layout\includes\widget\card_calendar.pug
```puml
if theme.aside.card_calendar.enable
  .card-widget.card-calendar
    .card-content
      .item-headline
        i.fa.fa-calendar(aria-hidden="true")
        span= _p('aside.card_calendar')
      div.widget-wrap
      div#calendar.widget
```
- themes\butterfly\source\css\_layout\calendar.styl
```css
  #calendar
  a
  text-decoration none
   
  .cal-head
  position relative
  height 20px
  padding 8px 6px 2px 6px
  margin-bottom 15px
   
  .cal-prev,.cal-next
  position absolute
  top 9px
  width 9px
  height 10px
  padding 3px 4px
  border 1px solid transparent
  color #333
  outline 0
   
  .cal-prev
  left 8px
  &:before
  border-right 9px solid #333
   
  .cal-next
  right 8px
  &:before
  border-left 9px solid #333
   
  .cal-prev:before,.cal-next:before
  content ''
  display block
  width 0
  height 0
  border-top 5px solid transparent
  border-bottom 5px solid transparent
   
  .cal-title
  width 120px
  margin 0 auto
  color #333
  font bold 14px/18px Arial
  text-align center
  a
  border 1px solid transparent
  color color-theme
   
  .cal,
  .cal th,
  .cal td
  border none
   
  .cal
  border-collapse collapse
  border-spacing 0
  table-layout fixed
  width 100%
  margin 0
  th
  background white
  color black
  font-weight 900 !important
  tbody
  a
  background-color #42d3d8
  color white
  display block
  font-weight 700
  .cal-today
  background-color #eaf4f3
  color #26a2a4
  .cal-gray
  color #ddd
   
  .cal th,
  .cal td
  font-weight normal
  line-height 2.5625
  padding 0
  text-align center
   
  .cal-title a:hover,
  .cal-prev:hover,
  .cal-next:hover,
  .cal .cal-foot:hover,
  .cal .cal-foot:focus
  cursor pointer
   
  .cal tbody a:hover,
  .cal tbody a:focus
  background-color #A1E4E6
  color #fff
  cursor pointer
```
- 此外还要修改themes\butterfly\layout\includes\widget\index.pug文件，增加日历组件的显示位置
```puml
!=partial('includes/widget/card_calendar', {}, {cache: true})  ##找到类似的代码所在区域贴进去
```

## 运行
好了，到了这一步基本上已经完成日历组件的引入了，接下来就愉快的运行部署吧
```shell
hexo clean
hexo s
```
最后看一下效果  
![](https://s2.loli.net/2024/08/06/auEvpALzXrg4K7R.png)