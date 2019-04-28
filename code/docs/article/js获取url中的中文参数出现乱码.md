---
classify: JS及jQuery
tags: [JavaScript, 坑]
---

## js 获取 url 中的中文参数出现乱码

之前从 url 中获取参数遇到过中文变成乱码的格式，之后从网上找了一些资料发现了原因，先上代码：

```
function getRequest() {
       var url = window.location.search; //获取url
       var theRequest = new Object();  //新建对象用来保存参数
       if (url.indexOf("?") != -1) {  //判断url是否有？
          var str = url.substr(1);  //获取？后面的字符串如id=1&name=111之类的
          strs = str.split("&");  //根据&把字符串分着成数组
          for(var i = 0; i < strs.length; i ++) {
             theRequest[strs[i].split("=")[0]]=decodeURI(strs[i].split("=")[1]); //再把如id=1形式的变成id:1
          }
       }
       return theRequest;
    }
```

关于防止中文变成乱码，就是用 decodeURI()；
看下介绍：
**decodeURI() 函数可对 encodeURI() 函数编码过的 URI 进行解码。**
而 encodeURI()我搜了下**encodeURI() 函数可把字符串作为 URI 进行编码。**
看看的效果

```
var test1="http://www.w3school.com.cn/My first/我的"
document.write(encodeURI(test1)+ "<br>")//http://www.w3school.com.cn/My%20first/%E6%88%91%E7%9A%84
document.write(decodeURI(test1))//http://www.w3school.com.cn/My first/我的
```

所以解析 url 参数出现中文乱码，可以看看是不是解码出了问题。
