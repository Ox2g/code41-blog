title: "Markdown常用语法"
date: 2015-03-24 07:46:18
tags: 
- 基础
- 工具
- markdown
categories: 
- 基础知识
---

## 文字基础

\*斜体\* *斜体*  
\_斜体\_ _斜体_ 
\*\*加粗\*\* **加粗**   
\_\_加粗\_\_ __加粗__ 

this is `important` text. \`important\`

## 缩进
> 缩进
>> 缩进缩进
>>> 123

## 标题
# Big title (h1)
## Middle title (h2)
### Smaller title (h3)
#### and so on (hX)
##### and so on (hX)
###### and so on (hX)

## 列表

 - bullets can be `-`, `+`, or `*`
 - bullet list 1
 - bullet list 2
    - sub item 1
    - sub item 2

        with indented text inside

 - bullet list 3
 + bullet list 4
 * bullet list 5

## 超链接

This is an [example inline link](http://lmgtfy.com/) and [another one with a title](http://lmgtfy.com/ "Hello, world").

## 图片

A sample image :

![revolunet logo](http://www.revolunet.com/static/parisjs8/img/logo-revolunet-carre.jpg "revolunet logo")



## Code

使用 ___\`\`\`__  对代码类型进行声明

```java
public Class Test extends BaseTest implements BaseInterface {
    private static String testStr = "Hello World";

    public static void main (String[] args) {
        System.out.println(testStr);
    }
} 

```

Some Javascript code :

```js
var config = {
    duration: 5,
    comment: 'WTF'
}
// callbacks beauty un action
async_call('/path/to/api', function(json) {
    another_call(json, function(result2) {
        another_another_call(result2, function(result3) {
            another_another_another_call(result3, function(result4) {
                alert('And if all went well, i got my result :)');
            });
        });
    });
})
```

## 表格

### 表格的用法

| Year | Temperature (low) | Temperature (high) |  
| ---- | ----------------- | -------------------|  
| 1900 |               -10 |                 25 |  
| 1910 |               -15 |                 30 |  
| 1920 |               -10 |                 32 |  

