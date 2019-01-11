title: 前端工具函数utils.js
author: 徐子玉
tags:
  - javascript
categories:
  - 前端
date: 2018-10-24 14:44:00
---
> 这个utils.js主要用于layui中  
<!-- more -->

```
/*
 * 扩展一个utils模块
 * */
layui.define(['layer', 'table', 'form', 'laydate', 'laytpl', 'admin', 'view'], function (exports) { //提示：模块也可以依赖其它模块，如：layui.define('layer', callback);
  var $ = layui.$,
    layer = layui.layer,
    form = layui.form,
    table = layui.table,
    laydate = layui.laydate,
    laytpl = layui.laytpl,
    admin = layui.admin,
    view = layui.view,
    setter = layui.setter,
    baseUrl = setter.baseUrl;
  var utils = {
    // 测试
    hello: function (str) {
      console.log('Hello ' + (str || "xuqi's m-utils"));
    },
    /*时间转换*/
    checkTime: function checkTime(i) {
      if (i < 10) {
        i = '0' + i;
      }
      return i;
    },
    formatTime: function formatTime(date) {
      var getFullYear = this.checkTime(date.getFullYear());
      var getMonth = this.checkTime(date.getMonth() + 1);
      var getDate = this.checkTime(date.getDate());
      var getHours = this.checkTime(date.getHours());
      var getMinutes = this.checkTime(date.getMinutes());
      var getSeconds = this.checkTime(date.getSeconds());
      return getFullYear + "-" + getMonth + "-" + getDate + " " + getHours + ":" + getMinutes + ":" + getSeconds;
    },
    
    /* 开发中的提示 */
    coding: function (btn) {
      var btn_dom = btn;
      if (!btn) {
        btn_dom = '.js-btn-coding';
      }
      $(btn_dom).on('click', function () {
        layer.alert('维护中，需官方授权开放');
        return false;
      });
    },
    codingAlert: function () {
      layer.alert('维护中，需官方授权开放');
      return false;
    },
    /* 配置table */
    setTableOption: function (dataName, method) {
      var method = method || 'get';
      table.set({
        method: method,
        request: {
          pageName: 'curpage', //页码的参数名称，默认：page
          limitName: 'pagesize' //每页数据量的参数名，默认：limit
        },
        response: {
          statusName: 'code', //数据状态的字段名称，默认：code
          statusCode: 200, //成功的状态码，默认：0
          msgName: 'message', //状态信息的字段名称，默认：msg
          countName: 'count', //数据总数的字段名称，默认：count
          dataName: dataName //数据列表的字段名称，默认：data
        }
      })
    },
    /*类型验证*/
    checkStr: function (str, type) {
      switch (type) {
        case 'phone':
          //手机号码
          return (/^1[3|4|5|7|8][0-9]{9}$/.test(str));
        case 'tel':
          //座机
          return (/^(0\d{2,3}-\d{7,8})(-\d{1,4})?$/.test(str));
        case 'card':
          //身份证
          return (/^\d{15}|\d{18}$/.test(str));
        case 'pwd':
          //密码以字母开头，长度在6~18之间，只能包含字母、数字和下划线
          return (/^[a-zA-Z]\w{5,17}$/.test(str));
        case 'postal':
          //邮政编码
          return (/[1-9]\d{5}(?!\d)/.test(str));
        case 'QQ':
          //QQ号
          return (/^[1-9][0-9]{4,9}$/.test(str));
        case 'email':
          //邮箱
          return (/^[\w-]+(\.[\w-]+)*@[\w-]+(\.[\w-]+)+$/.test(str));
        case 'money':
          //金额(小数点2位)
          return (/(^[1-9]([0-9]+)?(\.[0-9]{1,2})?$)|(^(0){1}$)|(^[0-9]\.[0-9]([0-9])?$)/.test(str)
            // return (/^\d*(?:\.\d{0,2})?$/.test(str)
          );
        case 'URL':
          //网址
          return (/(http|ftp|https):\/\/[\w\-_]+(\.[\w\-_]+)+([\w\-\.,@?^=%&:/~\+#]*[\w\-\@?^=%&/~\+#])?/.test(str));
        case 'IP':
          //IP
          return (/((?:(?:25[0-5]|2[0-4]\\d|[01]?\\d?\\d)\\.){3}(?:25[0-5]|2[0-4]\\d|[01]?\\d?\\d))/.test(str));
        case 'date':
          //日期时间
          return (/^(\d{4})\-(\d{2})\-(\d{2}) (\d{2})(?:\:\d{2}|:(\d{2}):(\d{2}))$/.test(str) || /^(\d{4})\-(\d{2})\-(\d{2})$/.test(str));
        case 'number':
          //数字
          return (/^[0-9]$/.test(str));
        case 'integers':
          //数字
          return (/^-?[1-9]\d*$/.test(str));
        case 'integer':
          //非负整数
          return (/^(0|[1-9]\d*)$/.test(str));
        case 'integer5':
          //非负整数1-5
          return (/^[0-5]$/.test(str));
        case 'english':
          //英文
          return (/^[a-zA-Z]+$/.test(str));
        case 'chinese':
          //中文
          return (/^[\u4E00-\u9FA5]+$/.test(str));
        case 'lower':
          //小写
          return (/^[a-z]+$/.test(str));
        case 'upper':
          //大写
          return (/^[A-Z]+$/.test(str));
        case 'HTML':
          //HTML标记
          return (/<("[^"]*"|'[^']*'|[^'">])*>/.test(str));
        default:
          return true;
      }
    },
    positiveInteger: function () {
      form.verify({
        positiveInteger: [
          /^(0|[1-9]\d*)$/, '请输入正整数'
        ]
      });
    },
    money: function () {
      form.verify({
        money: [
          /(^[1-9]([0-9]+)?(\.[0-9]{1,2})?$)|(^(0){1}$)|(^[0-9]\.[0-9]([0-9])?$)/, '金额只能保留小数点后两位'
        ]
      });
    },
    /*保留两位小数*/
    returnFloat: function returnFloat(value) {
      // var value=Math.round(parseFloat(value)*100)/100;
      var xsd = value.toString().split(".");
      if (xsd.length == 1) {
        value = value.toString() + ".00";
        return value;
      }
      if (xsd.length > 1) {
        if (xsd[1].length < 2) {
          value = value.toString() + "0";
        } else {
          value = xsd[0] + '.' + xsd[1].substr(0, 2);
        }
        return value;
      }
    },
    /*选择时间范围*/
    laydate: function (dom) {
      laydate.render({
        elem: dom, //指定元素
        range: true, //或 range: '~' 来自定义分割字符
        done: function (value, date, endDate) {
          var start_date, end_date;
          var $start = $(dom).siblings('.js-time-start');
          var $end = $(dom).siblings('.js-time-end');
          // console.log(value);
          if (value) {
            start_date = date.year + '-' + utils.checkTime(date.month) + '-' + utils.checkTime(date.date);
            end_date = endDate.year + '-' + utils.checkTime(endDate.month) + '-' + utils.checkTime(endDate.date);
          } else {
            start_date = '';
            end_date = '';
          }
          console.log({
            'start_date': start_date,
            'end_date': end_date
          });
          $start.val(start_date);
          $end.val(end_date);
        }
      });
    },
    /* 可能废弃，layer有photos方法可替换 */
    showImg: function showImg($this, td_title_index) {
      let idx = td_title_index;
      let type = typeof (idx);
      // let this_id = '#' + $this.attr('id');
      let this_class = '.' + $this.attr('class');
      let td_img_index = $this.closest('td').index();
      let th_img_title = $this.closest('.layui-table-box').find('.layui-table-header thead>tr>th').eq(td_img_index).text();
      let content_img = $this.find('img');
      let th_index_title = '';
      console.log(type)
      if (idx && type === 'number') {
        th_index_title = $this.closest('tr').find('td').eq(idx).text();
        th_img_title = th_index_title + '的' + th_img_title;
      } else if (idx && type === 'string') {
        th_index_title = $this.closest('tr').find('td[data-field="' + idx + '"]').text();
        th_img_title = th_index_title + '的' + th_img_title;
      }
      // console.log(idx, this_str);
      layer.open({
        type: 1,
        shadeClose: true,
        scrollbar: false,
        title: th_img_title || '图像',
        content: $this, //捕获的元素，注意：最好该指定的元素要存放在body最外层，否则可能被其它的相对元素所影响
        // area: ['500px', '542px'],
        success: function () {
          $('body').off('click', this_class);
        },
        end: function () {
          $('body').on('click', this_class, function () {
            var $this = $(this);
            console.log(idx);
            utils.showImg($this, idx);
          });
        }
      });
    },
    /*展示图片新方法*/
    photos: function (domClass) {
      var imgParent = $('img').parent();
      if (domClass) {
        imgParent = $(`.${domClass}`);
      }
      imgParent.each(function (i) {
        let id = $(this).attr('id');
        if (id) {
          layer.photos({
            photos: `#${id}`,
            anim: 5
          })
        }
      });
    },
    /* 关闭layer的iframe窗口 */
    closeIframe: function () {
      var index = parent.layer.getFrameIndex(window.name);
      parent.layer.close(index)
    },
    /* 获取url参数，返回值为object */
    args: function (params) {
      var a = {};
      params = params || location.search;
      if (!params) return {};
      params = decodeURI(params);
      params.replace(/(?:^\?|&)([^=&]+)(?:\=)([^=&]+)(?=&|$)/g, function (m, k, v) {
        a[k] = v;
      });
      return a;
    },
    /* 重置form表单方法，不会重置部分input的值 */
    reset: function ($reset) {
      var $input = $($reset).closest('.layui-form').find(":input");
      // var $select = $($reset).closest('.layui-form').find("select");
      // console.log($input.not(":button, :submit, :reset,:disabled,[readonly]"));
      $input.not(":button, :submit, :reset,:disabled,[readonly]").val("");
      $input.not(":disabled,[readonly]").removeAttr("checked");
    },
    /*模板渲染*/
    tplRender: function (tplId, viewId, tplData, callback) {
      var $tplId, $viewId;
      if (typeof (tplId) === 'string') {
        if (tplId.indexOf('#') === -1) {
          $tplId = document.getElementById(tplId);
        } else {
          $tplId = document.getElementById(tplId.substring(1));
        }
      }
      if (typeof (viewId) === 'string') {
        if (viewId.indexOf('#') === -1) {
          $viewId = document.getElementById(viewId);
        } else {
          $viewId = document.getElementById(viewId.substring(1));
        }
      }
      var getTpl, view;
      if ($tplId) {
        getTpl = $tplId.innerHTML;
      } else {
        getTpl = tplId.innerHTML;
      }
      if ($viewId) {
        view = $viewId;
      } else {
        view = viewId;
      }
      var data = tplData || true;
      // console.log(data)
      laytpl(getTpl).render(
        data,
        function (html) {
          view.innerHTML = html;
          callback && callback();
        }
      );

    },
    /* 给没有http开头的图片修改成完整的图片地址 */
    ossUrl: 'http://garment-scm.oss-cn-hangzhou.aliyuncs.com/',
    imgUrl: function (img, size, url) {
      let newUrl = img;
      let baseUrl = url || this.ossUrl;
      let urlAfter = size || '';
      if (size === 'sm') {
        urlAfter = '?x-oss-process=image/resize,w_132,h_132';
      }
      if (size === 'lg') {
        urlAfter = '?x-oss-process=image/resize,w_800,h_800';
      }
      if (img.indexOf('http') === -1) {
        // newUrl=baseUrl+newUrl;
        newUrl = baseUrl + newUrl + urlAfter;
      }
      return newUrl;
    },
    /* 给没有http开头的网址添加成完整的网址 */
    httpUrl: function (url) {
      let newUrl = url;
      if (url.indexOf('http') === -1) {
        newUrl = 'http://' + newUrl;
      }
      return newUrl;
    },
    strSubstr(str, index) {
      let num = this.ossUrl.length;
      let idx = index || 0;
      let newStr = str.substr(idx, num)
    },
    /* 把图片名称组成的字符串转换成数组Array */
    strToArr: function (str, separator) {
      var splitStr = separator || '|';
      var Arr = [];
      if (str) {
        Arr = str.split(splitStr)
      }
      return Arr;
    },
    /* 把图片名称组成的数组Array转换成字符串String */
    arrToStr: function (arr, separator) {
      var splitStr = separator || '|';
      var Str = arr.join(splitStr);
      if (arr.length === 1) {
        Str = arr[0]
      }
      if (arr.length === 0) {
        Str = ''
      }
      return Str;
    },
    /*数组去重*/
    arrNoRep: function (arr) {
      for (var i = 0; i < arr.length; i++) {
        if (arr.indexOf(arr[i]) != i) {
          arr.splice(i, 1);//删除数组元素后数组长度减1后面的元素前移
          i--;//数组下标回退
        }
      }
      return arr;
    },
    /*数组相减*/
    arrMinus: function (a, b) {
      let lg=[],sm=[];
      if(a.length>=b.length){
        lg=a.concat();
        sm=b.concat();
      }else{
        lg=b.concat();
        sm=a.concat();
      }
      // console.log(lg,sm)
      for (var i = lg.length - 1; i >= 0; i--) {
        a = lg[i];
        for (var j = sm.length - 1; j >= 0; j--) {
          b = sm[j];
          if (a == b) {
            lg.splice(i, 1);
            sm.splice(j, 1);
            break;
          }
        }
      }
      return lg;
    },
    /*添加图片*/
    _addItimImg: function ({elemId, hiddenId, list, picture, one = true}) {
      let that = this;
      // console.log('_addItimImg:',$(hiddenId).val());
      let valArr = utils.strToArr($(hiddenId).val());

      valArr[valArr.length] = picture;
      let valNew = utils.arrToStr(valArr);
      let $list = $(elemId).closest('.js-upload-group').find(list);

      let newId = 'id' + (new Date().getTime());
      let $item = `<div class="layui-upload-item">
                      <div class="layui-upload-item__child" id="${newId}">
                      <img layer-src="${this.imgUrl(picture)}" src="${this.imgUrl(picture)}">
                      </div>
                      <a href="javascript:;" class="layui-upload-img-del js-upload-img-del">删除</a>
                    </div>`;
      if (one) {
        $list.html($item);
        valNew = picture
      } else {
        $list.append($item);
      }

      $(hiddenId).val(valNew);
      layer.msg('上传成功', {
        icon: 1,
        time: 800
      }, function () {
        that.photos();
      });
    },
    /*删除图片列表中的图片*/
    _delItemImg: function ({$this, hiddenId, item}) {
      let that = this;
      // const itemId = $($this).closest(item).attr('id');
      layer.confirm('确定要删除该图片吗？', {
        icon: 2
      }, function (index) {
        layer.close(index);
        let thisFullSrc = $($this).closest(item).find('img').attr('src');
        console.log(thisFullSrc)
        let thisSrc = utils.strSubstr(thisFullSrc);
        let imgStr = $(hiddenId).val();
        let imgArr = utils.strToArr(imgStr);
        let arrIndex = $.inArray(thisSrc, imgArr);
        imgArr.splice(arrIndex, 1);
        let newImgStr = utils.arrToStr(imgArr);
        $(hiddenId).val(newImgStr)
        $($this).closest(item).remove();
      })
    },
    /*自定义input验证*/
    verify: function () {
      form.verify({
        nickname: function (value, item) { //value：表单的值、item：表单的DOM对象
          if (!new RegExp("^[a-zA-Z0-9_\u4e00-\u9fa5\\s·]+$").test(value)) {
            return '用户名不能有特殊字符';
          }
          if (/(^\_)|(\__)|(\_+$)/.test(value)) {
            return '用户名首尾不能出现下划线\'_\'';
          }
          if (/^\d+\d+\d$/.test(value)) {
            return '用户名不能全为数字';
          }
        }

        //我们既支持上述函数式的方式，也支持下述数组的形式
        //数组的两个值分别代表：[正则匹配、匹配不符时的提示文字]
        ,
        pass: [
          /^[\S]{6,12}$/, '密码必须6到12位，且不能出现空格'
        ]

        //确认密码
        ,
        repass: function (value) {
          if (value !== $('#newPassword').val()) {
            return '两次密码输入不一致';
          }
        },
        positiveInteger: [
          /^(0|[1-9]\d*)$/, '请输入正整数'
        ],
        money: [
          /(^[1-9]([0-9]+)?(\.[0-9]{1,2})?$)|(^(0){1}$)|(^[0-9]\.[0-9]([0-9])?$)/, '金额只能保留小数点后两位'
        ]
      });
    },
    /* 过滤掉formData的空值 */
    formDataFilter: function (object) {
      for (key in object) {
        if (object[key] === '') {
          delete object[key]
        }
      }
      return object
    },
    /* 获取table数据,在回调函数callback里渲染table */
    tableReq: function (options) {
      let formData = options.formData,
        url = `${baseUrl}${options.url}`,
        type = options.type || 'GET',
        dataType = options.dataType || 'json',
        data = this.formDataFilter(options.data),
        callback = options.callback;
      admin.req({
        url: url,
        type: type,
        dataType: dataType,
        data: data,
        done: function (res) {
          callback && callback(res);
        }
      })
    },
    /* 判断小屏，弹出层全屏 */
    full: function (layerName) {
      let screenNum = admin.screen();
      if (screenNum === 0) {
        layer.full(layerName);
      }
    }
  };

  /* 清空form数据 */

  var $dateRest = $('.layui-form [type="reset"]');

  $($dateRest).on('click', function () {
    utils.reset($(this));
    form.render();
    return false;
  });

  // 输出test接口
  exports('utils', utils);
});
```