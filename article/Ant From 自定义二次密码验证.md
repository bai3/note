# Ant Form 自定义二次密码验证

> Ant pro 输入框 rules的  validator使用



主要使用两个方法

- getFieldValue

  获取到第一个输入框的值进行比较

- validator

  自定义校验的方法(注意无论什么结果都要callback出来)

  使用方法 ` function(rule, value, callback)`

### 具体实现代码

```javascript
rules: [
    { required: true,message: '请再次输入新密码(6~20位数字、字母或下划线)'},
    { validator(rule, value, callback){
        if(!value){
            callback()//如果还没填写，则不进行一致性验证
        }
        if(value == getFieldValue('password')){
            callback()
        }
        else{
            callback('两次密码不一致')
        }
    }}
]
```

![](..\images\#27.png)