title: javascript工具函数
author: 徐子玉
tags:
  - javascript
categories:
  - 项目实战
date: 2018-03-05 16:58:00
---
### 前端获取url参数

```
function getQueryString(name) {
        var reg = new RegExp("(^|&)" + name + "=([^&]*)(&|$)", 'i'); // 匹配目标参数
        var result = window.location.search.substr(1).match(reg); // 对querystring匹配目标参数
        if (result != null) {
            return decodeURIComponent(result[2]);//对 encodeURIComponent() 函数编码的 URI 进行解码。
        } else {
            return null;
        }
    }
```
调用函数获取url参数

	var user = parseInt(getQueryString('user'),10)||null;//获取user的参数并转换成10进制数字

### jquery的$.ajax()设置

	$.ajaxSetup({
        beforeSend:function(){
            //loading带文字
            $.showLoading();
        },
        error: function (err) {
            console.log("发生错误：", err)
            $.hideLoading();
        },
        dataFilter:function (data,type) {
            $.hideLoading();
            return data;
        }
    });