title: 极验证-geetest-前端配置
author: 徐子玉
tags:
  - 插件
  - 验证
categories:
  - 前端
date: 2018-03-02 16:57:00
---
在公司项目《玩客小兵》中，登陆或者注册的场景中需要进行`人机验证`，使用了第三方插件[极验证-geetest](http://www.geetest.com/)。
> 插件效果 <img src='http://image.xuziyu.cn/wx_20180302160419.png' alt='极验证插件效果' style="width:250px;" /> 
***
<!-- more -->
### 安装
1. 引入初始化函数  
```
<script src="gt.js"></script>
```
2. 调用初始化函数进行初始化

        $.ajax({
          // 获取id，challenge，success（是否启用failback）
          url: "/user/startCaptchaServlet?t=" + (new Date()).getTime(), // 后台接口，加随机数防止缓存
          type: "get",
          dataType: "json",
          success: function (data) {
              console.log(data);
              // 使用initGeetest接口
              // 参数1：配置参数
              initGeetest({
                  gt: data.gt,
                  challenge: data.challenge,
                  new_captcha: data.new_captcha,
                  product: "embed", // 产品形式，包括：float，embed，popup。注意只对PC版验证码有效
                  offline: !data.success // 表示用户后台检测极验服务器是否宕机，一般不需要关注
              }, 
              // 参数2：回调，回调的第一个参数验证码对象，之后可以使用它做appendTo之类的事件
              handlerEmbed);
          }
        });

3. 初始化成功后，使用回调函数  

		var handlerEmbed = function (captchaObj) {
        var this_data = {
            mobile: null,
            type: 'signup'
        };
        //点击下一步
        $(".js-next-login").click(function (e) {
            $('.geetest_btn').trigger('click');
        });
        // 监听验证成功事件
        captchaObj.onSuccess(function () {
            //如果验证成功，就执行相关代码
        });
        // 将验证码加到id为captcha的元素里，同时会有三个input的值：geetest_challenge, geetest_validate, geetest_seccode
        captchaObj.appendTo("#embed-captcha");
        captchaObj.onReady(function () {
        });
    	};