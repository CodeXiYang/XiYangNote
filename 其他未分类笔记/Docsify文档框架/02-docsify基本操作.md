# Docsify基本操作





## 初始化Doscify

进入当前文件夹夹执行`doscify init`进行项目初始化

![image-20201019151439050](assets/image-20201019151439050.png)





## 启动Doscify

启动doscify服务`doscify serve`



## Doscify基本配置



### 加载中

在文件数量过大时,想包含一个提示加载中可以在div里面编写,div的id需要通过`el`挂载到script标签里面

```html
<body>
  <div id="xiyang">
    当前正在加载中...
  </div>
  <script>
    window.$docsify = {
      el:"#xiyang",
      name: '',
      repo: ''
    }
  </script>
  <script src="//cdn.jsdelivr.net/npm/docsify/lib/docsify.min.js"></script>
</body>
```

### 多页文档

当访问子文件夹的时候默认是访问该文件下的`README.md`文件





### 定制侧边栏

开启`loadSidebar: true`

```html
  <script>
    window.$docsify = {
      el:"#xiyang",
      loadSidebar: true, /*开启侧边栏定制*/
      name: '',
      repo: '',
    }
  </script>
```

在当前文件夹里面创建一个文件`_sidebar.md`并写上如下内容

```markdown
* [遇见狂神说java系列学习](狂神说系列笔记/)

* [Redis5.0.8](【狂神说Java】Redis最新超详细版教程通俗易懂/)
```

*ps: 当定制侧边栏后,无法显示文章的标题侧边栏目,不推荐*



### 显示文章标题

开启`subMaxLevel: 6`

```html
  <script>
    window.$docsify = {
      el:"#xiyang",
      loadSidebar: true, /*开启侧边栏定制*/
      subMaxLevel: 6, /*自定义侧边栏后默认不会再生成目录，你也可以通过设置生成目录的最大层级开启这个功能。*/
      name: '',
      repo: '',
    }
  </script>
```



### 文档封面

首先需要先开启封面主题

```html
  <script>
    window.$docsify = {
      el:"#xiyang",
      loadSidebar: true, /*开启侧边栏定制*/
      subMaxLevel: 6, /*自定义侧边栏后默认不会再生成目录，你也可以通过设置生成目录的最大层级开启这个功能。*/
      coverpage: true, //启用封面页。开启后是加载 _coverpage.md 文件，也可以自定义文件名。
      name: '',
      repo: '',
    }
  </script>
```

然后在当前目录下创建一个文件`_coverpage.md` 编写以下内容;注意带`<>`是实心按钮,不带的是空心按钮

```markdown
# 遇见狂神说学习笔记

- 本文档是根据B站UP主 "遇见狂神说Java" 上传的java视频学习所做的笔记

[![stars](https://badgen.net/github/stars/Q-Angelo/Nodejs-Roadmap?
icon=github&color=4ab8a1)](https://github.com/Q-Angelo/Nodejs-Roadmap) [![forks] (https://badgen.net/github/forks/Q-Angelo/Nodejs-Roadmap?icon=github&color=4ab8a1)] (https://github.com/Q-Angelo/Nodejs-Roadmap)

[Github](地址) [xiyang](地址) [开始阅读](地址)
```

### 文档主题

doscify默认提供的是vue样式的主题,可以在index.html进行引入切换

```html
<head>
  <meta charset="UTF-8">
  <title>Document</title>
  <meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1" />
  <meta name="description" content="Description">
  <meta name="viewport" content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
<!--  <link rel="stylesheet" href="//cdn.jsdelivr.net/npm/docsify/lib/themes/vue.css">--> <!--默认主题-->
  <link rel="stylesheet" href="//cdn.jsdelivr.net/npm/docsify/themes/dolphin.css"> <!--切换为其他主题-->
</head>
```

常用的几种主题如下:

```html
<link rel="stylesheet" href="//cdn.jsdelivr.net/npm/docsify/themes/vue.css">
<link rel="stylesheet" href="//cdn.jsdelivr.net/npm/docsify/themes/buble.css">
<link rel="stylesheet" href="//cdn.jsdelivr.net/npm/docsify/themes/dark.css">
<link rel="stylesheet" href="//cdn.jsdelivr.net/npm/docsify/themes/pure.css">
<link rel="stylesheet" href="//cdn.jsdelivr.net/npm/docsify/themes/dolphin.css">
```



###  搜索插件

index.html中的插件配置

```html
  <script>
    window.$docsify = {
      el:"#xiyang",
      loadSidebar: true, /*开启侧边栏定制*/
      subMaxLevel: 6, /*自定义侧边栏后默认不会再生成目录，你也可以通过设置生成目录的最大层级开启这个功能。*/
      coverpage: true, //启用封面页。开启后是加载 _coverpage.md 文件，也可以自定义文件名。
      name: '',
      repo: '',
      //搜索
      search: {
        maxAge: 86400000,//过期时间,单位毫秒,默认一天
        noData: "居然找不到搜索结果",//搜索不到结果时显示
        paths: "auto",//自动
        placeholder: "请搜索",//搜索框提示
      }
    }
  </script>
  <script src="//cdn.jsdelivr.net/npm/docsify/lib/docsify.min.js"></script>
  <!--搜索插件-->
  <script src="//cdn.jsdelivr.net/npm/docsify/lib/plugins/search.min.js"></script>
```

### 拷贝插件

添加下面的内容到index.html即可使用代码拷贝

```html
<script src="//unpkg.com/docsify-copy-code"></script>
```



## 更多学习

查看官网: https://docsify.js.org/#/zh-cn/
guide文章: https://blog.csdn.net/qq_34337272/article/details/105511189