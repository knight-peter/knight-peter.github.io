title: vue小白学习笔记0--购物车
author: 徐子玉
tags:
  - vue
  - vue小白学习笔记
categories:
  - vue
date: 2018-02-13 15:20:00
---
> 第一个实战：写一个购物车实例
> 为了从零开始学习，vue.js文件是用引入的。
> 用两个v-for实现两个商品组
<!-- more -->
#### html

    <div id="app" v-cloak>
        <template v-if="list.length">
          <table>
            <thead>
              <tr>
                <th>
                  <input type="checkbox" name="" id="" :checked="checkedAll" @click="checkAll">
                </th>
                <th>序号</th>
                <th>商品名称</th>
                <th>商品单价</th>
                <th>购买数量</th>
                <th>操作</th>
              </tr>
            </thead>
            <template v-for="(item,index) in newList">
              <tbody :key="index">
                <tr v-for="(item_child,idx) of item">
                  <td>
                  <!-- index,idx为参数 -->
                    <input type="checkbox" name="" id="" :checked="item_child.checked" @click="check(index,idx)">
                  </td>
                  <td>{{idx+1}}</td>
                  <td>{{item_child.name}}</td>
                  <td>{{item_child.price}}</td>
                  <td>
                    <button @click="handleReduce(idx)" :disabled="item_child.disabled">-</button>
                    <span>{{item_child.count}}</span>
                    <button @click="handleAdd(idx)">+</button>
                  </td>
                  <td>
                    <button @click="handleRemove(idx)">移除</button>
                  </td>
                </tr>
              </tbody>
              <hr>
            </template>
          </table>
          <div>总价：￥ {{totalPrice}} 元</div>
        </template>
        <div v-else>购物车为空</div>
      </div>
    ```
    #### script
    ```
    let app = new Vue({
     el: '#app',
     data: {
       checkedAll: true,
       list: [
         [{
             id: 1,
             name: 'iPhone 7',
             price: 1000,
             count: 1,
             checked: true,
           },
           {
             id: 2,
             name: 'iPad Pro',
             price: 2000,
             count: 1,
             checked: true,
           },
           {
             id: 1,
             name: 'MacBook Pro',
             price: 3000,
             count: 1,
             checked: true,
           },
         ],
         [{
             id: 1,
             name: '西瓜',
             price: 10,
             count: 1,
             checked: true,
           },
           {
             id: 2,
             name: '冬瓜',
             price: 20,
             count: 1,
             checked: true,
           },
           {
             id: 1,
             name: '南瓜',
             price: 30,
             count: 1,
             checked: true,
           },
           {
             id: 1,
             name: '黄瓜',
             price: 30,
             count: 1,
             checked: true,
           },
         ]
       ],

     },
     /* 计算属性 */
     computed: {
     //总价
       totalPrice: function () {
         let total = 0;
         for (let i = 0; i < this.list.length; i++) {
           let arr = this.list[i];
           for (let n = 0; n < arr.length; n++) {
             let arr_child = arr[n];
             if (arr_child.checked === true) {
               total += arr_child.price * arr_child.count;
             }
           }


         }
         // 返回添加千位分隔符的字符串
         return total.toString().replace(/\B(?=(\d{3})+$)/g, ',');
       },
       newList: function () {
         for (let i = 0; i < this.list.length; i++) {
           let item = this.list[i];
           // console.log(item);
           for (let n = 0; n < item.length; n++) {
             let item_list = item[n];
             if (item_list.count === 1) {
               item_list.disabled = true
             } else {
               item_list.disabled = false
             }
           }

         }
         return this.list;
       },
     },
     /* 方法 */
     methods: {
        //减少商品数量
       handleReduce: function (index) {
         if (this.list[index].count === 1) {
           return;
         }
         this.list[index].count--;
       },
       //增加商品数量
       handleAdd: function (index) {
         this.list[index].count++
       },
       //删除商品
       handleRemove: function (index) {
         this.list.splice(index, 1);
       },
       //全选，全不选
       checkAll: function () {

         this.checkedAll = !this.checkedAll;
         let this_checked;

         for (let i = 0; i < this.list.length; i++) {
           let item = this.list[i];
           for (let n = 0; n < item.length; n++) {
             let item_child = item[n];
             item_child.checked = this.checkedAll;
           }

         }
         console.log('全选：', this.checkedAll, ', this_checked:', this.checkedAll);
       },
       //选中商品，取消选中
       check: function (index, idx) {
         this.list[index][idx].checked = !this.list[index][idx].checked;
         for (let i = 0; i < this.list.length; i++) {
           let item = this.list[i];
           for (let n = 0; n < item.length; n++) {
             let item_child = item[n];
             if (item_child.checked === false) {
               this.checkedAll = false;
               return
             } else {
               this.checkedAll = true;
             }
           }

         }
         // console.log('改变后', this.newList);
       }
     },
     /*实例已挂载*/
     mounted: function () {
       // console.log(this.newList);
     }
    });