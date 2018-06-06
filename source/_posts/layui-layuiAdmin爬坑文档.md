title: layui&layuiAdmin 爬坑文档
author: 徐子玉
date: 2018-06-06 09:10:48
tags:

---

## table 数据更新，工具栏无法联动变化

---

> **原因**：table 对非对应 field 更新数据是无法响应的。没有 templet 字段就没法让 update()识别。

<!-- more -->

- 将工具栏和变动状态栏合并。缺点是**table 结构限制**，优点是**代码简单**。
  > 方案一 把工具栏和相关联的 field 的合并，工具栏需要添加 templet:#tool_ID 用来让 update()识别，从而更新完成,必须是 withdraw_state 这一列没有，可以有更好的展示效果

```javascript
<table id="tableAccount" lay-filter="tableAccount"></table>
<!-- 将提现状态和操作合并到一栏 -->
<script type="text/html" id="tableToolBar">
  {{# if(d.withdraw_state == 'default'){ }}
  <a class="layui-btn layui-btn-xs" lay-event="success">通过</a>
  <a class="layui-btn layui-btn-xs layui-btn-danger" lay-event="fail">驳回</a>
  {{# } else if(d.withdraw_state == 'success'){ }}
  <span class="layui-badge layui-bg-gray">已成功</span>
  {{# } else if(d.withdraw_state == 'fail'){ }}
  <span class="layui-badge layui-bg-gray">已失败</span>
  {{# } }}
</script>
```

```javascript
{
  field: 'withdraw_state',
  title: '操作',
  fixed: 'right',
  width: 180,
  align: 'center',
  templet: '#tableToolBar',
  toolbar: '#tableToolBar'
}
```

使用`.update()`更新 table 数据，然后工具栏会变化

```javascript
obj.update({
  withdraw_state: 'success',
  withdraw_time: time,
});
```

- 使用 layui 的 laytpl 来对工具栏进行更新。缺点是**代码量大，逻辑复杂**，优点是**适用性强**。
  > 方案二 使用 updata 更新 table,
  > 原因：因为不是对应 field 的数据，所以[data-field="toolStatus"]所在的工具条不支持更新， 1.只能通过使用 laytpl 获取到更新后的代码模板， 2.操纵 tr = obj.tr 找到 tool 对应的 field('td[data-field="toolStatus"]'),找到对饮 field 下面的的 div 用 html()更新 tool 内容 。

状态和操作是分开两列的。

```javascript
{
 field: 'withdraw_state',
  title: '提现状态',
  templet: '#withdraw_state'
},
{
 field: 'toolStatus',
  title: '操作',
  fixed: 'right',
  width: 180,
  align: 'center',
  templet: '#tableToolBar',
  toolbar: '#tableToolBar'
}
```

```javascript
var timestamp = new Date(1527837613 * 1000);
var time = utils.formatTime(timestamp);
obj.update({
  withdraw_state: 'success',
  withdraw_time: time,
});
data.withdraw_state = 'success';
laytpl(tableToolBar.innerHTML).render(obj.data, function(html) {
  //tableToolBar为toolbar的script模板id
  toolhtml = html;
});
tr.children('td[data-field="toolStatus"]')
  .children('div')
  .html(toolhtml); //toolStatus为当前表格工具列是表格的第几列
```

- 重载 table 的当前页，缺陷是**可能 table 的数据量变化，导致当前行不在当前页了**，优点是**代码量少**。
  > 方案三 使用`.layui-laypage-btn`_(表格分页中的确定按钮)_，重载当前页面

```javascript
$('.layui-laypage-btn').trigger('click');
```
