title: vue解决跨域问题
author: 徐子玉
tags:
  - 'vue,环境配置'
categories:
  - vue
date: 2018-04-03 14:05:00
---
* 如果把所有的接口，统一规范为一个入口，在一定程度上会解决冲突  

        proxyTable: {
              '/api/**': { //使用"/api"来代替"源地址"
                target: 'http://www.qiangchebao.cn/ver100', //源地址
                changeOrigin: true, //改变源
                pathRewrite: {
                  '^/api': '/' //路径重写
                }
              }
            },
 
 <!-- more -->
*  上面这个代码，就是把咱们虚拟的这个api接口，去掉，此时真正去后端请求的时候，不会加上api这个前缀了，那么这样我们前台http请求的时候，还必须加上api前缀才能匹配到这个代理,代码如下： 

		logout(){
          axios.post('/api/users/logout').then(result=>	{
              let res = result.data;
              this.nickName = '';
              console.log(res);
          	})
      	},

* 我们可以利用`axios的baseUrl`直接默认值是 api，这样我们每次访问的时候，**自动补上这个api前缀**，就不需要我们自己手工在每个接口上面写这个前缀了  
**在入口文件里面配置如下：**

        import Axios from 'axios'
        import VueAxios from 'vue-axios'

        Vue.use(VueAxios, Axios)
        Axios.defaults.baseURL = 'api'

        //如果这配置 'api/' 会默认读取本地的域  
        
 > vue-axios的用法  

         Vue.axios.get(api).then((response) => {
          console.log(response.data)
        })

        this.axios.get(api).then((response) => {
          console.log(response.data)
        })

        this.$http.post(api,{params}).then((response) => {
          console.log(response.data)
        })
> 在使用post方式的时候传递参数有两种方式，一种是**普通的formed方式**，一种是**json方式**，如果后台接受的是普通方式，那么使用上述方式即可。  

**普通的formed方式**

默认情况下，axios串联js对象为 JSON 格式。使用 `application/x-www-form-urlencoded` 格式化   
**浏览器 Browser** 
在浏览器中你可以如下使用 `URLSearchParams API`:  
```
var params = new URLSearchParams();
params.append('param1','value1');
params.append('param2','value2');
axios.post('/foo',params);
```

> 注意： URLSearchParams 不支持所有的浏览器  

其他方法：你可以使用 qs 库来格式化数据。

    var qs = require('qs');
    axios.post('/foo', qs.stringify({'bar':123}));  
    
**json方式**

    data: {'bar':123}  
* 上面这样配置的话，不会区分生产和开发环境，在config 文件夹里面新建一个 `api.config.js` 配置文件，用来分别设置不同环境。  

        const isPro = Object.is(process.env.NODE_ENV, 'production')

        module.exports = {
          baseUrl: isPro ? 'http://www.qiangchebao.cn/ver100' : 'api/'
        }  
然后在 `main.js` 里面引入,这样可以保证动态的匹配生产和开发的定义前缀  

		//引入api.config.js
      import apiConfig from '../config/api.config'

      import Axios from 'axios'
      import VueAxios from 'vue-axios'

      Vue.use(VueAxios, Axios)
      //根据环境设置axios的baseurl
      Axios.defaults.baseURL = apiConfig.baseUrl    
经过上面配置后，在dom里面可以这样轻松的访问,也不需要在任何组件里面引入axios模块了。

* 在dom里面请求api的姿势  

      this.$http
            .post(
              "/car/getCarDetail",
              qs.stringify({
                car_id: carId
              })
            )
            .then(res => {
            	console.log(res);
            });