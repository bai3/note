## url转换成二维码并提供下载

> 工作需求，需要将url转换成二维码，并提供一个直接下载的按钮。

#### 首先

使用jQuery的qrcode插件将一个url转换成一个二维码

html部分代码

```html
<div id="invite-qrcode"></div>
<img id="qrcode-logo" src="../qrcode-logo.png" style="display:none">//这个用于生成二维码中心的图片
```

js代码

```javascript
$("#invite-qrcode").qrcode({
    render: 'canvas', //生成canvas格式图片，也可以选择"table"生成div格式
    text: str, // url地址
    mode: 4,
    ecLevel: 'Q',
    mSize: 0.3,
    mPosX: 0.5,
    mPosY: 0.5,
    image: $('#qrcode-logo')[0] //中心图片
}); 
```

#### 然后

使用jquery在canvas后面动态生成一个供下载点击的按钮

#### 最后

添加按钮上的点击函数

```javascript
$('.image-btn').click(function(e){
    var index = $(this).attr('num'); //按钮上的序列
    // 使用toDataURL方法将图像转换被base64编码的URL字符串
    var url = $('#invite-qrcode canvas')[index].toDataURL('image/png')
    // 将a的download属性设置为我们想要下载的图片名称，若name不存在则使用‘二维码’作为默认名称
    $(this).attr("download",'二维吗')
    // 将生成的URL设置为a.href属性
    $(this).attr("href", url) 
 })
```

点击下载按钮就可以下载相应的二维码图片。

