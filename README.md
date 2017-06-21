# Multi-level-select
a vue component to select an item from multi-level data

### 简介 
一个多级选择组件。目前支持至多三级。目前组件只支持从众多选项中选择一项。
为了支持三级层次的选择，组件内部递归调用了组件自身。当点击一个选项后。会执行使用设定的回调函数。

### 使用
```
<multi-level-select :data-source="positionArray" @select="select" v-show="show"></multi-level-select>
```
