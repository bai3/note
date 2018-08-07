## word-wrap
> word-wrap属性允许长单词或URL地址换行到下一行

- normal
  只在允许的断字点换行
- break-word
  在长单词或URL地址内部进行换行

## word-break
> word-break 属性指定非CJK脚本的断行规则
> CJK脚本是中国，日本和韩国脚本

- normal
  使用浏览器默认的换行规则
- break-all
  允许在单词内换行
- keep-all
  只能在半角空格或连字符处换行

## white-space
> white-space 属性设置如何处理元素内的空白

- normal
  默认,空白会被浏览器忽略
- pre
  空白会被浏览器保留
- nowrap
  文本不会换行，文本会在同一行上继续，直到遇`<br />` 标签为止
- pre-wrap
  保留空白符序列，但是正常地进行换行
- pre-line
  合并空白符序列，但是保留换行符