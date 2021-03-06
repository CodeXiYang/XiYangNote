# Docsify文档框架(完结)

> 视频: https://www.bilibili.com/video/BV1jD4y1d7uF
>
> 说明: 学习的目的是用于记录笔记,比较喜欢docsify的文档风格

## 1. Docsify简介和安装

### 1.1 简介

`docsify`是由现饿了么前端团队@elemeFE的cinwell.li编写的一套文档站点生成框架，github上已有3k+ star,Docsify 是一个动态生成文档网站的工具。不同于 GitBook、Hexo 的地方是它不会生成将
.md 转成 .html 文件，所有转换工作都是在运行时进行。

### 1.2 特性

- 无需构建，写完文档直接发布

- 容易使用并且轻量 (~19kB gzipped)

- 智能的全文搜索(`搜索插件`)

- 提供多套主题

- 丰富的 API(`各种插件`)

- 支持 Emoji(`emoji表情大全`)

- 兼容 IE10+

- 支持 SSR (**example**)

### 1.3 安装

#### 1.3.1 安装node.js

![](assets/image-20201019145919405.png)

#### 1.3.2 npm安装doscify框架

推荐全局安装 `docsify-cli` 工具，可以方便地创建及在本地预览生成的文档。

```bash
npm i docsify-cli -g
```

### 1.4 样板

*其他人基于docsify框架记录笔记的样板*

https://veal98.gitee.io/cs-wiki/#/

## 2. Docsify基本操作

### 2.1 初始化Doscify

进入当前文件夹夹执行`doscify init`进行项目初始化

![image-20201019151439050](assets/image-20201019151439050.png)

### 2.2 启动Doscify

启动doscify服务`doscify serve`

### 2.3 Doscify基本配置

#### 2.3.1 加载中

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

#### 2.3.2 多页文档

当访问子文件夹的时候默认是访问该文件下的`README.md`文件

#### 2.3.3 定制侧边栏

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

#### 2.3.4 显示文章标题

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

#### 2.3.5 文档封面

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

#### 2.3.6 文档主题

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

#### 2.3.7 搜索插件

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

#### 2.3.8 拷贝插件

添加下面的内容到index.html即可使用代码拷贝

```html
<script src="//unpkg.com/docsify-copy-code"></script>
```

## 3. 更多学习

查看官网: https://docsify.js.org/#/zh-cn/

Guide哥: https://blog.csdn.net/qq_34337272/article/details/105511189

## 4. 个性化设置

*记录下自己调试出来的docsify样式代码记录*

### index.html

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>XiYang  Note</title>
  <meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1" />
  <meta name="description" content="Description">
  <meta name="viewport" content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
  <!--默认主题-->
  <link rel="stylesheet" href="//cdn.jsdelivr.net/npm/docsify/lib/themes/vue.css">
  <!-- <link rel="stylesheet" href="resource/css/myStyle-light.css">--> <!--本主题在vue.css基础上进行了自定义-->
  <!----><link rel="stylesheet" href="resource/css/xiyang.css">
  <!--评论样式-->
  <link rel="stylesheet" href="//cdn.jsdelivr.net/npm/gitalk/dist/gitalk.css">
  <!--网站icon图标-->
  <link rel="shortcut icon" href="resource/favicon.ico" type="image/x-icon" />
</head>
<body>
  <!--加载提示-->
  <div id="xiyang">
    🏃‍🏃‍🏃‍💨 疯狂加载中...
  </div>
  <!--回到顶部-->
  <a class="to-top"> 🚀 </a>
  <script>
    window.$docsify = {
      //开启参数
      el:"#xiyang",
      loadSidebar: true, //开启侧边栏选项
      subMaxLevel: 6, //自定义侧边栏后默认不会再生成目录，你也可以通过设置生成目录的最大层级开启这个功能.
      coverpage: true, //启用封面页。开启后是加载 _coverpage.md 文件，也可以自定义文件名。
      name: 'XiYang Note',
      repo: '',
      loadNavbar: true,
      auto2top:true, //切换页面后是否自动跳转到页面顶部
      onlyCover: true,
      topMargin: 140,
      disqus: 'shortname',//开启评论
      //搜索插件
      search: {
        paths: 'auto',
        placeholder: '🔍 搜索',
        noData: '😒 找不到结果',
        depth: 6, // Headline depth, 1 - 6
        maxAge: 86400000, // 过期时间，单位毫秒，默认一天
      },
      //网站底部信息
      footer: {
        copy: '🚀 XiYang &copy; 2019 - 2022</span>',
        auth: ' <strong> CodeXiYang </strong>',
        pre: '<hr/>',
        style: 'text-align: center;font-size:0.1rem;position:relative;top:2rem',
      },
      //代码拷贝插件
      copyCode: {
        buttonText: '复制',
        errorText: '错误',
        successText: '复制成功!'
      },
      count:{
        countable:true,
        fontsize:'0.9em',
        color:'rgb(221,27,27)',
        language:'chinese'
      }
    }
  </script>

  <!--docsify框架-->
  <script src="//cdn.jsdelivr.net/npm/docsify/lib/docsify.min.js"></script>
  <!--搜索插件-->
  <script src="https://cdn.bootcss.com/docsify/4.5.9/plugins/search.min.js"></script>
  <!--emjio插件-->
  <script src="//cdn.jsdelivr.net/npm/docsify/lib/plugins/emoji.min.js"></script>
  <!--字数统计-->
  <script src="//unpkg.com/docsify-count/dist/countable.js"></script>
  <!--代码拷贝-->
  <script src="//cdn.jsdelivr.net/npm/docsify-copy-code"></script>
  <!--评论插件-->
  <script src="//cdn.jsdelivr.net/npm/docsify/lib/plugins/disqus.min.js"></script>
  <!-- CDN files for docsify-katex -->
  <script src="//cdn.jsdelivr.net/npm/docsify-katex@latest/dist/docsify-katex.js"></script>
  <script src="//unpkg.com/docsify-footer-enh/dist/docsify-footer-enh.min.js"></script>
  <!-- 访问量统计 -->
  <script async src="//busuanzi.ibruce.info/busuanzi/2.3/busuanzi.pure.mini.js"></script>
  <!-- 回到顶部插件 -->
  <script src="resource/js/jquery-1.11.3.min.js"></script>
  <script src="resource/js/jquery.toTop.min.js"></script>
  <script>
    $('.to-top').toTop();
  </script>
  <!--------------------------------Gitalk评论插件------------------------------->
<!--
  <script src="//cdn.jsdelivr.net/npm/docsify/lib/plugins/gitalk.min.js"></script>
  <script src="//cdn.jsdelivr.net/npm/gitalk/dist/gitalk.min.js"></script>
  <script>
 var gitalkConfig = {
    		clientID: '577942f4612618483174', //上面生成的clientID
    		clientSecret: 'f52a3ee26b2b796006b5efe2713fb1927080c549',//上面生成的clientSecret
    		repo: 'XiYangNote',//Github博客项目名
    		owner: 'CodeXiYang',//github用户名
    		admin: [],//github用户名，可以添加多个
    		distractionFreeMode: false
	   }
	    window.$docsify = {
            loadSidebar: true,
            plugins: [
              // 评论会自动同步成为项目的issue,
              // 这些代码是给issue添加lable以及更改样式
              // 我的博客是取每个路由地址的第一级目录名称作为label，你可以自己操作路径自己自定义
                function (hook, vm) {
                    hook.doneEach(function () {
                        var label='', domObj, main, divEle, gitalk;
                        var labels = vm.route.path.split('/');
                        if(labels.length) {
                            for (let i = 0; i < labels.length; i++) {
                                var v = labels[i]
                                if(v && v !=='README') {
                                    label = v
                                    break;
                                }
                            }
                        }
                        domObj = Docsify.dom;
                        main = domObj.getNode("#main");
                        Array.apply(
                            null,
                            document.querySelectorAll("div.gitalk-container")
                        ).forEach(function (ele) {
                            ele.remove();
                        });

                        divEle = domObj.create("div");
                        divEle.id = "gitalk-container-" + label;
                        divEle.className = "gitalk-container";
                        divEle.style = "width: " + main.clientWidth + "px; margin: 0 auto 20px;";
                        domObj.appendTo(domObj.find(".content"), divEle);
                        gitalk = new Gitalk(
                            Object.assign(gitalkConfig, {id: !label ? "home" : label})
                        );
                        gitalk.render("gitalk-container-" + label);
                    });
                }
            ]
    }
  </script>
-->

<!--  &lt;!&ndash;分页导航:不太好用&ndash;&gt;-->
<!--  <script src="//cdn.jsdelivr.net/npm/docsify/lib/docsify.min.js"></script>-->
<!--  <script src="//cdn.jsdelivr.net/npm/docsify-pagination/dist/docsify-pagination.min.js"></script>-->

  <!--  &lt;!&ndash; 复制提醒 &ndash;&gt;-->
  <!--  <script src="https://cdn.bootcss.com/sweetalert/1.1.3/sweetalert.min.js"></script>-->
  <!--  <script>-->
  <!--    document.body.oncopy = function () {-->
  <!--      swal("复制成功 🎉",-->
  <!--              "若要转载或引用请务必保留原文链接，并申明来源。如果你觉得本仓库不错，那就来 Github 给个 star 吧 😊",-->
  <!--              "success"); };-->
  <!--  </script>-->


  <!--代码高亮
  高亮网址: https://cdn.jsdelivr.net/npm/prismjs/components/
  -->
  
  <script src="//cdn.jsdelivr.net/npm/prismjs/components/prism-bash.min.js"></script> <!--bash-->
  <script src="//cdn.jsdelivr.net/npm/prismjs/components/prism-java.min.js"></script> <!--java-->
  <script src="//cdn.jsdelivr.net/npm/prismjs/components/prism-python.min.js"></script> <!--python-->
  <script src="//cdn.jsdelivr.net/npm/prismjs/components/prism-cpp.min.js"></script>
  <script src="//cdn.jsdelivr.net/npm/prismjs/components/prism-c.min.js"></script> <!--c-->
  <script src="//cdn.jsdelivr.net/npm/prismjs/components/prism-javascript.min.js"></script> <!--javascript-->
  <script src="//cdn.jsdelivr.net/npm/prismjs/components/prism-css.min.js"></script> <!--css-->
  <script src="//cdn.jsdelivr.net/npm/prismjs/components/prism-powershell.min.js"></script> <!--shell-->
  <script src="//cdn.jsdelivr.net/npm/prismjs/components/prism-sql.min.js"></script> <!--sql-->
  <script src="//cdn.jsdelivr.net/npm/prismjs/components/prism-yaml.min.js"></script> <!--yaml-->
  <script src="//cdn.jsdelivr.net/npm/prismjs/components/prism-properties.min.js"></script> <!--properties-->
  <script src="//cdn.jsdelivr.net/npm/prismjs/components/prism-ini.min.js"></script> <!--ini-->
  <script src="//cdn.jsdelivr.net/npm/prismjs@1.22.0/components/prism-json.min.js"></script> <!--json-->
  <script src="//cdn.jsdelivr.net/npm/prismjs@1.22.0/components/prism-jsonp.min.js"></script> <!--jsonp-->
  <script src="//cdn.jsdelivr.net/npm/prismjs@1.22.0/components/prism-nginx.min.js"></script> <!--nginx-->
  <script src="//cdn.jsdelivr.net/npm/prismjs@1.22.0/components/prism-mongodb.min.js"></script> <!--mongodb-->
  <script src="//cdn.jsdelivr.net/npm/prismjs@1.22.0/components/prism-markdown.min.js"></script> <!--markdown-->
  <script src="resource/js/jquery-1.11.3.min.js"></script>
  <script src="resource/js/jquery.toTop.min.js"></script>
  <script src="resource/js/xiyangnote.js"></script>



</body>
</html>

<!--
其他编写式例
1. https://github.com/Sanarous/Sanarous.github.io/blob/master/index.html
-->
```

### README.css

```css
h2{
    /* border-bottom:2px  solid   rgb(66, 185, 131); */
    margin-bottom:50px;
    font-size: 1em;
}
h2 span{
    display:inline-block;
    background: #4f9ee3;
    color:#ffffff !important;
    padding:  10px  16px;
    border-radius:5px;
    box-shadow: 2px 2px 5px rgb(216, 216, 216);
}
/* .content{
  width:1000px;
  margin: 0 auto;
  padding-top: 30px;
} */
.markdown-section{
    padding: 30px 30px 40px 30px !important;
}
table{
    box-sizing: border-box;
}
.markdown-section th{
    border:none;
}
.markdown-section td{
    padding: 30px;
}
.markdown-section td:hover{
    background-color: #d9d7d7;
    box-sizing: border-box;
    cursor: pointer;
}
```

### myStyle-light.css

```css
/*myStyle-light.css*/
/**
=================IT`S ME 定制!===============
=================XiYang======================
*/
a {
    text-decoration: none;
}
body{
    font-family: "Helvetica Neue",Helvetica,"Hiragino Sans GB","Microsoft YaHei",Arial,sans-serif;   
    word-spacing:1px; /*更改默认的词间距*/
    letter-spacing:1px;/*更改默认的字符间距*/ 
    /* background:rgb(255, 255, 255); */

}
/* 置顶插件 */
.to-top {
    display: none;
    padding: 10px 20px;
    margin-right: 10px;
    color: #ffffff;
    border-radius: 10px;
    background-color: rgb(66, 185, 163);
    background: linear-gradient(right,rgb(66 185 163), rgb(102, 126, 233),rgb(66 185 163));
    background: -ms-linear-gradient(right,rgb(66 185 163), rgb(102, 126, 233),rgb(66 185 163));
    background: -webkit-linear-gradient(right,rgb(66 185 163), rgb(102, 126, 233),rgb(66 185 163));
    background: -o-linear-gradient(right,rgb(66 185 163), rgb(102, 126, 233),rgb(66 185 163));
    background: -moz-linear-gradient(right,rgb(66 185 163), rgb(102, 126, 233),rgb(66 185 163));
    text-align: center;
    vertical-align: middle;
}
.to-top:hover{
    background: -webkit-linear-gradient(right,rgb(102, 126, 233),rgb(66 185 163),rgb(102, 126, 233));
}
/* 首页a标签置顶,不然无法点击 */
section.cover .cover-main {
    z-index:10 !important;
}
/* 导航栏 */
/* .app-nav {
    margin: 20px 60px 0 0 !important;
} */
/* 导航栏字体颜色 */
/* .app-nav ul, .app-nav li {
    color: #eee !important;
} */
/* 导航栏下拉框背景 */
/* .app-nav li ul {
    background-color: rgb(68, 68, 68) !important;
} */
/* 内容居中和宽度 */
.content{
    width:1000px;
    margin: 0 auto;
    padding-top: 70px !important;
}
/* 内容区域的字体颜色和背景 */
.markdown-section{
    /* background:rgb(247, 247, 247); */
    /* color: rgb(184, 191, 198); */
    color: black;
    max-width: 90% !important;
    padding: 30px 30px 40px 30px !important;
    border-radius: 20px;
}
/* 行代码块 */
.markdown-section code {
    font-size: 14px;
    font-family: Consolas, monospace, serif;
    white-space: nowrap;
    padding: 0 4px;
    border-radius: 3px;
    background: rgb(247, 247, 247);
    white-space: normal;
    color: #f22f27;
    
}
/* 搜索框 */
/* .search {
    border-bottom: 1px solid rgb(68,68,68) !important;
}
.search input {
    background:rgb(51, 51, 51);
    color: rgb(184, 191, 198)
}
.search a {
    color: rgb(184, 191, 198) !important;
} */

/* 侧边栏背景颜色 */
/* .sidebar{
    background:rgb(51, 51, 51);
} */
/* 侧边栏字体颜色 */
.sidebar ul li a {
    /* color: rgb(222, 222, 222); */
    font-weight: 600;
    font-size:16px;
}
.sidebar ul ul li a {
    /* color: rgb(184, 191, 198); */
    font-weight:400 !important;
    font-size:14px !important;
}

.sidebar ul li.active > a {
    color: rgb(66, 185, 163);
} 

/* 侧边栏标题 */
.sidebar > h1 a{
    color: black
}
/* 侧边栏和内容分界线 */
.sidebar {
    border-right: 1px solid rgb(247, 247, 247);
}

/* .sidebar-toggle{
    background-color: rgb(68,68,68) !important;
} */
h1
{
    font-weight: bold !important;
    color:#ffffff !important;
}
h1 span{
    display:inline-block;
    background: rgb(102, 126, 233);
    color:#ffffff !important;
    padding:  10px  16px;
    border-radius:5px;
    box-shadow: 2px 2px 5px rgb(216, 216, 216);
}
.markdown-section hr {
    border: none;
    border-bottom: 2px solid rgb(247, 247, 247) !important;
    margin: 2em 0;
}
/* 标题颜色 */
.markdown-section h2 span, .markdown-section h3 span, .markdown-section h4 span,.markdown-section h5 span{
    /* color: rgb(222, 222, 222); */
    font-weight: bold;
}


/* 封面半透明遮盖 */
section.cover.has-mask .mask {
    background-color: #fff;
    opacity: 0;
    position: absolute;
    top: 0;
    height: 100%;
    width: 100%;
}

/* 加粗 */
.markdown-section strong {
    color: rgb(102, 126, 233);
}
/* a 标签 */
.markdown-section a {
    color: rgb(66, 185, 163);
    font-weight: 500;
}
/* 表格 */
.markdown-section tr:nth-child(2n) {
    background-color: rgb(51, 51, 51);
}
/* 引用块 */
.markdown-section blockquote{
    background: rgb(220, 220, 255);
    padding:10px;
    border-radius:10px;
}
/* 代码块 */
/* .markdown-section pre {
    box-shadow: 2px 2px 20px 6px #ddd !important;
} */

/* 代码块 */
.markdown-section pre:before {
    content: '';
    display: block;
    background: url(https://my-wechat.mdnice.com/point.png);
    height: 30px;
    width: 100%;
    background-size: 50px;
    background-repeat: no-repeat;
    background-color: #282c34;
    margin-bottom: -50px;
    margin-top: 10px;
    margin-left: -3px;
    border-radius: 5px;
    background-position: 10px 10px;
}
/* 代码块 */
.markdown-section pre>code {
    color: #c0c3c1 !important;
    font-family: 'Inconsolata', consolas,"PingFang SC", "Microsoft YaHei", monospace !important;
    background-color: #282c34 !important;    
    font-size: 15px !important;
    white-space: pre !important;
    line-height: 1.5 !important;
    -moz-tab-size: 4 !important;
    -o-tab-size: 4 !important;
    tab-size: 4 !important;

}
/* 代码块 */
markdown-section code, .markdown-section pre {
    background-color: #282c34;
}
.token.directive.keyword{
    color: #4faee2 !important;
}

.token.keyword{
    color: #c678dd !important;
}

.token.comment{
    color: #737c8b !important;
}

.token.tag{
    color: #a589ad !important;
}


.token.attr-name{
    color: #de916c !important;
}
.token.attr-value{
    color: #4faee2 !important;
}

.token.macro.property{
    color: #4faee2 !important;
}

.token.function{
    color: #e6b456 !important;
}
.token.string{
    color: #98b755 !important;
}
.token.punctuation{
    color: #c0c3c1 !important;
}

.token.number{
    color:#c0c3c1  !important;
}

img, pre {
    border-radius: 8px;
}
img{
    border: 1px solid #a4a2a2;
    padding: 3px;
}
/*表格*/
.markdown-section tr:nth-child(2n) {
    background-color: rgb(248, 246, 246) !important;
}

/**/
.app-nav{
    position: fixed;
}
.app-nav ul li a{
    font-weight: bold;
    color:rgb(66, 185, 163);
}
.app-nav ul li a:hover{
    color:rgb(221,27,27);
}
/*文章中的a链接标签样式*/
#main p a{
    color:#F22F27;
    /*text-decoration:underline wavy rgb(66, 185, 163) ;*/
}
/*复选框的样式,做任务列表的*/
input[type="checkbox"] {
    width: 13px;
    height: 13px;
    display: inline-block;
    text-align: center;
    vertical-align: middle;
    line-height: 18px;
    margin-right: 10px;
    position: relative;
}
input[type="checkbox"]::before {
    content: "";
    position: absolute;
    top: -1px;
    left: -1px;
    background: #fff;
    width: 100%;
    height: 100%;
    border: 1px solid rgb(66, 185, 163);
    border-radius: 3px;
}
input[type="checkbox"]:checked::before {
    content: "✔️";
    background-color: #F22F27;
    position: absolute;
    top: -1px;
    left: -1px;
    width: 117%;
    border: 1px solid #F22F27;
    border-radius:3px;
    color: green;
    font-size: 10px;
    font-weight: bold;
}
/*侧边栏样式*/
body.sticky .sidebar, body.sticky .sidebar-toggle {
    position: fixed;
    background-color: #f4f4f4;
}
/*搜索框圆角*/
.search input {
    border-radius: 1rem;
}
/*首页*/
.app-nav a.active{
	border-bottom:none;
}
/*文章一级标题*/
.markdown-section h1 {
    font-size: 2rem;
    margin: 0 0 1rem;
    text-align: center;
}
```

## 5. 其他个性化设置

*由于之前弄的,好像有一些bug,然后就采用了github上的其他人样式了!!~~*

然后[参考](https://github.com/Sanarous/Sanarous.github.io/blob/master/index.html)结合之前的做了一些修改

### index.html

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>XiYang Note</title>
    <meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1"/>
    <meta name="description" content="Description">
    <meta name="viewport"
          content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
    <link rel="stylesheet" href="https://cyc-1256109796.cos.ap-guangzhou.myqcloud.com/vue.css">
    <link rel="stylesheet" href="//unpkg.com/gitalk/dist/gitalk.css">
    <!-- 自定义样式 -->
	<link rel="stylesheet" href="resource/css/xiyang.css">
	<!--网站icon图标-->
    <link rel="shortcut icon" href="resource/favicon.ico" type="image/x-icon" />
</head>
<body>
<div id="app">Loading...</div>
<!--回到顶部-->
<a class="to-top" style="font-size: 20px"> 🚀 </a>
<!--docsify配置js-->
<script>
    window.$docsify = {
        maxAge: 100,
        //名字配置
        name: 'XiYang Note',
        coverpage: true,
        //右上角github图标仓库地址配置
        repo: 'https://gitee.com/codexiyang/XiYangNote',
        //搜索配置
        search: {
            maxAge: 86400000, // 过期时间，单位毫秒，默认一天
            paths: 'auto',
            placeholder: '🔍 搜索 ',
            noData: '😞 找不到相关内容呢！',
            depth: 6
        },
        subMaxLevel: 2,
        // 加载 _navbar.md配置
        loadNavbar: true,
		//字数统计配置
		count:{
			countable:true,
			fontsize:'0.9em',
			color:'rgb(90,90,90)',
			language:'chinese'
		},
    };
</script>

<!--docsify框架所需要的一些配置js-->
<script src="//cdn.jsdelivr.net/npm/docsify/lib/docsify.min.js"></script>
<script src="https://cyc-1256109796.cos.ap-guangzhou.myqcloud.com/docsify-copy-code.min.js"></script>
<script src="https://cyc-1256109796.cos.ap-guangzhou.myqcloud.com/prism-java.min.js"></script>
<script src="https://cyc-1256109796.cos.ap-guangzhou.myqcloud.com/prism-c.min.js"></script>
<script src="https://cyc-1256109796.cos.ap-guangzhou.myqcloud.com/prism-bash.min.js"></script>
<script src="https://cyc-1256109796.cos.ap-guangzhou.myqcloud.com/prism-sql.min.js"></script>
<script src="//unpkg.com/docsify-edit-on-github/index.js"></script>
<script src="https://cyc-1256109796.cos.ap-guangzhou.myqcloud.com/zoom-image.min.js"></script>
<script src="https://unpkg.com/docsify-footer-enh/dist/docsify-footer-enh.min.js"></script>
<script src="//unpkg.com/docsify/lib/plugins/gitalk.min.js"></script>
<script src="//unpkg.com/gitalk/dist/gitalk.min.js"></script>
<script src="//unpkg.com/docsify-count/dist/countable.js"></script><!--字数统计插件-->
<script src="https://cdn.bootcss.com/docsify/4.5.9/plugins/search.min.js"></script> <!--搜索插件-->

<!--代码高亮网址: https://cdn.jsdelivr.net/npm/prismjs/components/-->
<script src="//cdn.jsdelivr.net/npm/prismjs/components/prism-bash.min.js"></script> <!--bash-->
<script src="//cdn.jsdelivr.net/npm/prismjs/components/prism-java.min.js"></script> <!--java-->
<script src="//cdn.jsdelivr.net/npm/prismjs/components/prism-python.min.js"></script> <!--python-->
<script src="//cdn.jsdelivr.net/npm/prismjs/components/prism-cpp.min.js"></script>
<script src="//cdn.jsdelivr.net/npm/prismjs/components/prism-c.min.js"></script> <!--c-->
<script src="//cdn.jsdelivr.net/npm/prismjs/components/prism-javascript.min.js"></script> <!--javascript-->
<script src="//cdn.jsdelivr.net/npm/prismjs/components/prism-css.min.js"></script> <!--css-->
<script src="//cdn.jsdelivr.net/npm/prismjs/components/prism-powershell.min.js"></script> <!--shell-->
<script src="//cdn.jsdelivr.net/npm/prismjs/components/prism-sql.min.js"></script> <!--sql-->
<script src="//cdn.jsdelivr.net/npm/prismjs/components/prism-yaml.min.js"></script> <!--yaml-->
<script src="//cdn.jsdelivr.net/npm/prismjs/components/prism-properties.min.js"></script> <!--properties-->
<script src="//cdn.jsdelivr.net/npm/prismjs/components/prism-ini.min.js"></script> <!--ini-->
<script src="//cdn.jsdelivr.net/npm/prismjs@1.22.0/components/prism-json.min.js"></script> <!--json-->
<script src="//cdn.jsdelivr.net/npm/prismjs@1.22.0/components/prism-jsonp.min.js"></script> <!--jsonp-->
<script src="//cdn.jsdelivr.net/npm/prismjs@1.22.0/components/prism-nginx.min.js"></script> <!--nginx-->
<script src="//cdn.jsdelivr.net/npm/prismjs@1.22.0/components/prism-mongodb.min.js"></script> <!--mongodb-->
<script src="//cdn.jsdelivr.net/npm/prismjs@1.22.0/components/prism-markdown.min.js"></script> <!--markdown-->
<script src="//cdn.jsdelivr.net/npm/prismjs@1.22.0/components/prism-markdown.min.js"></script> <!--markdown-->

<script src="resource/js/jquery-1.11.3.min.js"></script>
<script src="resource/js/jquery.toTop.min.js"></script><!-- 回到顶部插件,需要jQuery的支持 -->


<!--其他外部js-->
<script>
	//gitalk评论
	const gitalk = new Gitalk({
		clientID: '577942f4612618483174',
		clientSecret: 'f52a3ee26b2b796006b5efe2713fb1927080c549',
		repo: 'XiYangNote',
		owner: 'CodeXiYang',
		admin: ['CodeXiYang'],
		id: location.pathname,
		distractionFreeMode: false
	});
	//回到顶部
	$('.to-top').toTop();
</script>
</body>
</html>
```

### xiyang.css

```css
/*隐藏头部的目录 */
#main > ul:nth-child(1) {
	display: none;
}

#main > ul:nth-child(2) {
	display: none;
}

#main {
	max-width: 1100px;
}

h1 + ul {
	display: block !important;
}

/*        .markdown-section{
			width: 1000px !important;
		}*/

.markdown-section h1 {
	margin: 3rem 0 2rem 0;
}

.markdown-section h2 {
	margin: 2rem 0 1rem;
}

img,
pre {
	border-radius: 8px;
}

.content,
.sidebar,
.markdown-section,
body,
.search input {
	background-color: rgba(243, 242, 238, 1) !important;
}

.content {
	font-color: #000;
	letter-spacing: 2px;
}

@media (min-width: 600px) {
	.sidebar-toggle {
		background-color: #f3f2ee;
	}
}

.docsify-copy-code-button {
	background: #f8f8f8 !important;
	color: #7a7a7a !important;
}

body {
	/*font-family: Microsoft YaHei, Source Sans Pro, Helvetica Neue, Arial, sans-serif !important;*/
}

.markdown-section > p {
	font-size: 16px !important;
}

.markdown-section pre > code {
	font-family: Consolas, Roboto Mono, Monaco, courier, monospace !important;
}

@media (min-width: 600px) {
	.markdown-section pre > code {
		font-size: .9rem !important;
	}
}

@media (max-width: 600px) {
	.markdown-section pre > code {
		padding-top: 5px;
		padding-bottom: 5px;
	}

	pre:after {
		content: "" !important;
	}
}


/*.anchor span {
color: rgb(66, 185, 131);
}*/

section.cover h1 {
	margin: 0;
}

body > section > div.cover-main > ul > li > a {
	color: #42b983;
}

.markdown-section > div > img,
.markdown-section pre {
	box-shadow: 2px 2px 20px 6px #ddd !important;
}

pre {
	background-color: #f3f2ee !important;
}

@media (min-width: 600px) {
	pre code {
		padding-left: 20px !important;
	}
}

@media (max-width: 787px) {
	.content {
		letter-spacing: 0px !important;
	}

	.github-corner svg {
		display: none !important;
	}
}

@media (max-width: 600px) {
	pre {
		padding-left: 0px !important;
		padding-right: 0px !important;
	}

	.docsify-copy-code-button {
		display: none;
	}
}

.markdown-section pre {
	padding-left: 0 !important;
	padding-right: 0px !important;
}



code[class*="language-"],
pre[class*="language-"] {
	color: black;
	background: none;
	font-family: Consolas, Monaco, 'Andale Mono', 'Ubuntu Mono', monospace;
	text-align: left;
	white-space: pre;
	word-spacing: normal;
	word-break: normal;
	word-wrap: normal;
	line-height: 1.5;

	-moz-tab-size: 4;
	-o-tab-size: 4;
	tab-size: 4;

	-webkit-hyphens: none;
	-moz-hyphens: none;
	-ms-hyphens: none;
	hyphens: none;
}

/* Code blocks */
pre[class*="language-"] {
	position: relative;
	margin: .5em 0;
	overflow: visible;
	padding: 0;
}

pre[class*="language-"] > code {
	position: relative;
	border-left: 10px solid #358ccb;
	box-shadow: -1px 0px 0px 0px #358ccb, 0px 0px 0px 1px #dfdfdf;
	background-color: #fdfdfd;
	background-image: linear-gradient(transparent 50%, rgba(69, 142, 209, 0.04) 50%);
	background-size: 3em 3em;
	background-origin: content-box;
	background-attachment: local;
}

code[class*="language"] {
	max-height: inherit;
	height: inherit;
	padding: 0 1em;
	display: block;
	overflow: auto;
}

/* Margin bottom to accommodate shadow */
:not(pre) > code[class*="language-"],
pre[class*="language-"] {
	background-color: #fdfdfd;
	-webkit-box-sizing: border-box;
	-moz-box-sizing: border-box;
	box-sizing: border-box;
	margin-bottom: 1em;
}

/* Inline code */
:not(pre) > code[class*="language-"] {
	position: relative;
	padding: .2em;
	border-radius: 0.3em;
	color: #c92c2c;
	border: 1px solid rgba(0, 0, 0, 0.1);
	display: inline;
	white-space: normal;
}

pre[class*="language-"]:before,
pre[class*="language-"]:after {
	content: '';
	z-index: -2;
	display: block;
	position: absolute;
	bottom: 0.75em;
	left: 0.18em;
	width: 40%;
	height: 20%;
	max-height: 13em;
	box-shadow: 0px 13px 8px #979797;
	-webkit-transform: rotate(-2deg);
	-moz-transform: rotate(-2deg);
	-ms-transform: rotate(-2deg);
	-o-transform: rotate(-2deg);
	transform: rotate(-2deg);
}

:not(pre) > code[class*="language-"]:after,
pre[class*="language-"]:after {
	right: 0.75em;
	left: auto;
	-webkit-transform: rotate(2deg);
	-moz-transform: rotate(2deg);
	-ms-transform: rotate(2deg);
	-o-transform: rotate(2deg);
	transform: rotate(2deg);
}

/*黑色*/
.token.function,
.token.operator,
.token.class-name {
	color: #2C3E50;
}

/*深蓝加粗*/
.token.keyword {
	color: #124363;
	font-weight: bold;
}


/*浅蓝*/
.token.property,
.token.tag,
.token.boolean,
.token.number,
.token.function-name,
.token.constant,
.token.symbol,
.token.deleted {
	color: #2980B9;
}

.token.comment,
.token.block-comment,
.token.prolog,
.token.doctype,
.token.cdata {
	color: #7D8B99;
}

.token.punctuation {
	color: #5F6364;
}


.token.selector,
.token.attr-name,
.token.string,
.token.char,
.token.builtin,
.token.inserted {
	color: #1ABC9C;
	font-weight: bold;
}

.token.entity,
.token.url,
.token.variable {
	color: #a67f59;
	/*background: rgba(255, 255, 255, 0.5);*/
}

.token.atrule,
.token.attr-value {
	color: #1990b8;
}

.token.regex,
.token.important {
	color: #e90;
}

.language-css .token.string,
.style .token.string {
	color: #a67f59;
	background: rgba(255, 255, 255, 0.5);
}

.token.important {
	font-weight: normal;
}

.token.bold {
	font-weight: bold;
}

.token.italic {
	font-style: italic;
}

.token.entity {
	cursor: help;
}

.namespace {
	opacity: .7;
}

@media screen and (max-width: 767px) {

	pre[class*="language-"]:before,
	pre[class*="language-"]:after {
		bottom: 14px;
		box-shadow: none;
	}

}

/* Plugin styles */
.token.tab:not(:empty):before,
.token.cr:before,
.token.lf:before {
	color: #e0d7d1;
}

/* Plugin styles: Line Numbers */
pre[class*="language-"].line-numbers.line-numbers {
	padding-left: 0;
}

pre[class*="language-"].line-numbers.line-numbers code {
	padding-left: 3.8em;
}

pre[class*="language-"].line-numbers.line-numbers .line-numbers-rows {
	left: 0;
}

/* Plugin styles: Line Highlight */
pre[class*="language-"][data-line] {
	padding-top: 0;
	padding-bottom: 0;
	padding-left: 0;
}

pre[data-line] code {
	position: relative;
	padding-left: 4em;
}

pre .line-highlight {
	margin-top: 0;
}

::-webkit-scrollbar {
	width: 10px;
	height: 10px;
}

::-webkit-scrollbar-button {
	width: 0;
	height: 0;
}

::-webkit-scrollbar-button:start:increment, ::-webkit-scrollbar-button:end:decrement {
	display: none;
}

::-webkit-scrollbar-corner {
	display: block;
}

::-webkit-scrollbar-thumb {
	border-radius: 8px;
	background-color: rgba(0, 0, 0, .2);
}

::-webkit-scrollbar-thumb:hover {
	border-radius: 8px;
	background-color: rgba(0, 0, 0, .5);
}

::-webkit-scrollbar-track, ::-webkit-scrollbar-thumb {
	border-right: 1px solid transparent;
	border-left: 1px solid transparent;
}

::-webkit-scrollbar-track:hover {
	background-color: rgba(0, 0, 0, .15);
}

::-webkit-scrollbar-button:start {
	width: 10px;
	height: 10px;
	/*background: url(../images/scrollbar_arrow.png) no-repeat 0 0;*/
}

footer{
	display: none !important;
}
.app-nav{
	position: fixed;
	font-weight: bold;
}
section.cover.has-mask .mask {
	background-color: #101012!important;
}
```

