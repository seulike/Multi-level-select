# Multi-level-select
a vue component to select an item from multi-level data

### 简介 
一个多级选择组件。目前支持至多三级。目前组件只支持从众多选项中选择一项。
为了支持三级层次的选择，组件内部递归调用了组件自身。当点击一个选项后。会执行使用者设定的回调函数。  
组件具体样式如下:  
![img](https://github.com/seulike/Multi-level-select/blob/master/img/ex.png)

### 使用
```
<multi-level-select :data-source="positionArray" @select="select"></multi-level-select>
```
### 组件参数说明
<table>
<thead>
<tr><th>参数</th><th>说明</th><th>类型</th></tr>
</thead>
<tbody>
<tr>
<td>data-source</td>
<td>选择项展示数据</td>
<td>Array</td>
</tr>
</tbody></table>

### 组件事件说明
<table>
<thead>
<tr><th>事件名</th><th>说明</th><th>参数</th></tr>
</thead>
<tbody>
<tr>
<td>select</td>
<td>当用户手动勾选某一项时触发的事件</td>
<td>data</td>
</tr>
</tbody></table>
