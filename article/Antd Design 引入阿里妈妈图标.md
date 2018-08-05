## Ant Design如何引入阿里妈妈图标库的图标

> 在做ant为ui框架时，发现有些需要的icon，Ant Design框架并没有提供相应的图标，只能自己用阿里妈妈图标库进行扩展。

### 1. 阿里妈妈图标库

第一去阿里妈妈图标库创建自己的项目

![#37-1](https://github.com/bai3/note/blob/master/images/%2337-1.png?raw=true)

因为Ant的icon的class前缀是anticon，我们最好也社会中成anticon。Font Family随意。

接下来以Font class方式打包到本地，当然也可以用远程链接。

###2.Ant Design项目

![#37-2](https://github.com/bai3/note/blob/master/images/%2337-2.png?raw=true)

只需要这些文件，其他是demo介绍。引入项目中的资源文件夹。

然后创建一个common.less（我项目是less解析的）引入图标库

```less
// @import  "/iconfont/iconfont.css";//本地
@import "//at.alicdn.com/t/font_728915_ox6rcwcjtbn.css";//远程


/* 设置使用字体的优先级 anticon-user 高 */
:global(.anticon) {  /* :global() 是为了覆盖全局class .anticon 的样式 */
  &:before {
    /* 优先调用 anticon-user 字体，就是上面图例创建的 anticon-custom，名字对上就行，我自己的程序叫 anticon-user 了 */
    font-family: "anticon", "anticon-user" !important;   
    /* 默认样式是这样
        font-family: "anticon" !important;   
    */
  }
}
```

iconfont.css

```css

@font-face {font-family: "anticon-user";
  src: url('iconfont.eot?t=1530510094227'); /* IE9*/
  src: url('iconfont.eot?t=1530510094227#iefix') format('embedded-opentype'), /* IE6-IE8 */
  ...   
  url('iconfont.ttf?t=1530510094227') format('truetype'), /* chrome, firefox, opera, Safari, Android, iOS 4.2+*/
  url('iconfont.svg?t=1530510094227#anticon-user') format('svg'); /* iOS 4.1- */
}

.anticon-user {
  font-family:"anticon-user" !important;
  font-size:16px;
  font-style:normal;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

.anticon-jifen:before { content: "\e999"; }
```

然后在项目中应用Icon组件

即可正常使用

```html
<Icon type="jifen" />
```

**注意一下，icon的Unicode不能与Ant里重复，而且最好写比较后面，我试了一下，有时候填写不正确的话可能会导致显示不出来。**