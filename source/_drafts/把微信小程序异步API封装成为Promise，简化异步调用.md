title: 把微信小程序异步API封装成为Promise，简化异步调用
author: 徐子玉
date: 2018-09-14 17:10:00
tags:
---
> 把微信小程序异步API封装为Promise。用Promise处理异步操作会非常方便。

## 方法一
----
参考文章：[把微信小程序异步api转化为Promise，方便异步编程 ](https://github.com/tornoda/to-promise)
**核心代码**：
```
// index.js
const toPromise = (wx) => {
  return (method) => {
    return (option) => {
      return new Promise ((resolve, reject) => {
        wx[method]({
          ...option,
          success: (res) => { resolve(res) },
          fail: (err) => { reject(err) }
        })
      })
    }
  }
}

export default toPromise
```
#### 使用方法

1.在需要用到的地方引入  
```
 import toPromise from '/module/to-promise/src/index'
```
2.绑定微信全局对象`(wx)`到函数，以便可以取到微信得API
```
const toPromiseWx = toPromise(wx)
```
3.开始转化你需要得异步API
```
//apiName为微信异步方法名，如对wx.request()进行转化
const request = toPromiseWx('request')
//直接使用request方法
```
<!-- more -->
#### 举例
```
import toPromise from '/module/to-promise/src/index'

//转换wx.getStorage()
const getStorage = toPromsie(wx)('getStorage') 

//使用
getStorage({ key: 'test' })
  .then(
    (res) => {
      //res的值与wx.getStorage({ success: (res) => {} })中的res值一样
      //res = {data: 'keyValue'}
      console.log(res.data)//控制台打印storage中key对于的value
      return res.data//如果需要继续链式调用转化后的api，需要把值显示返回
    },
    (err) => {
      //err的值与wx.getStorage({ success: (err) => {} })中的err值一样
      throw err
    }
  )
```

## 方法二
----
参考文章：[微信小程序：使用Promise简化回调](https://segmentfault.com/a/1190000013150196)
因为微信小程序异步api都是success和fail的形式，所有有人封装了这样一个方法(**核心方法**):
```
//promisify.js
const promisify = (api) => {
    return (options, ...params) => {
        return new Promise((resolve, reject) => {
            api(Object.assign({}, options, { success: resolve, fail: reject }), ...params);
        });
    }
}
export default promisify
```
简单示例：
```
// 获取系统信息
wx.getSystemInfo({
    success: res => {
        // success
        console.log(res)
    },
    fail: res => {

    }
})
```
使用上面的`promisify.js`简化后:
```
import promisify from './promisify'
const getSystemInfo = promisify(wx.getSystemInfo)

getSystemInfo().then(res=>{
    // success
    console.log(res)
}).catch(res=>{

})
```
**复杂的例子**：
```
// 模拟获取code，然后将code传给后台，成功后获取userinfo，再将userinfo传给后台
// 登录
wx.login({
    success: res => {
        let code = res.code
        // 请求
        imitationPost({
            url: '/test/loginWithCode',
            data: {
                code
            },
            success: data => {
                // 获取userInfo
                wx.getUserInfo({
                    success: res => {
                        let userInfo = res.userInfo
                        // 请求
                        imitationPost({
                            url: '/test/saveUserInfo',
                            data: {
                                userInfo
                            },
                            success: data => {
                                console.log(data)
                            },
                            fail: res => {
                                console.log(res)
                            }
                        })
                    },
                    fail: res => {
                        console.log(res)
                    }
                })
            },
            fail: res => {
                console.log(res)
            }
        })
    },
    fail: res => {
        console.log(res)
    }
})
```
使用 `promisify.js` 简化后：
```
import promisify from './promisify'
const login = promisify(wx.login)
const getSystemInfo = promisify(wx.getSystemInfo)

// 登录
login().then(res => {
    let code = res.code
    // 请求
    pImitationPost({
        url: '/test/loginWithCode',
        data: {
            code
        },
    }).then(data => {
        // 获取userInfo
        getUserInfo().then(res => {
            let userInfo = res.userInfo
            // 请求
            pImitationPost({
                url: '/test/saveUserInfo',
                data: {
                    userInfo
                },
            }).then(data => {
                console.log(data)
            }).catch(res => {
                console.log(res)
            })
        }).catch(res => {
            console.log(res)
        })
    }).catch(res => {
        console.log(res)
    })
}).catch(res => {
    console.log(res)
})
```
> 可以看到简化效果非常明显，赶紧把小程序的异步调用API统统封装一遍，就可以愉快的开发小程序了。