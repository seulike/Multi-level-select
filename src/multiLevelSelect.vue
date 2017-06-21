<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

  .hide {
    display: none;
  }

  .hide.active {
    display: block;
  }

  .content {
    display: flex;
    transition: all 0.5s ease-out;
    height: 100%;
  }
  .left-nav {
    width: 7rem;
    height: 100%;
    max-height: 100%;
    overflow-y: scroll;
  }

  .right-content {
    flex: 1;
    max-height: inherit;
    overflow: hidden;
    overflow-y: scroll;
  }

  .left-nav>li,
  .right-content>ul>li {
    line-height: 3.8rem;
    padding-left: 1rem;
    position: relative;
    display: block;
    white-space: nowrap;
  }

  .left-nav>li {
    background-color: #f2f3f4;
  }

  .right-content>ul>li.active {
    color: #42b983;
  }

  .right-content>ul>li {
    background-color: #fff;
    border-bottom: 1px solid #ddd;
  }

  .left-nav>li.active{
    background: #fff;
  }

  .left-nav>li.light em {
    width: 0.8rem;
    height: 0.8rem;
    display: inline-block;
    border-radius: 50%;
    background: #ffa247;
    position: absolute;
    left: 0rem;
    top: 1.5rem;
  }

  .right-content>ul {
    height: 100%;
  }

</style>

<template>
  <div class="content" style="max-height: 100%;">
  <ul class="left-nav qt-br-x1">
  <li class="qt-bb-x1 qt-br-x1 business-nav" v-for="(items,index) in positions" :class="[index == chooseIndex ? 'active':'',items.hasItem ? 'light':'']" @click="choose($event,index)">{{items.name}}<em></em></li>
  </ul>
  <div class="right-content">
  <ul class="hide" v-for="(items,indexP) in positions" :class="indexP == chooseIndex ? 'active':''">
  <template v-if="items.selectMode === 1">
  <li v-for="info in items.positionArray" class="" :data-code="info.code" :class="info.code === items.chooseCode ? 'active':''" @click="select(indexP,$event,info)">
  <p class="qt-bb-x1 col10">{{info.name}}</p>
  </li>
  </template>
  <template v-else="items.selectMode === 2">
    <multi-level-select :data-source="items.positionArray" @select="select" :index-pra="indexP" ref="subFilter"></multi-level-select>
   </template>
  </ul>
  </div>
  </div>
</template>
<script>
import Vue from 'vue'
export default{
    name: 'multi-level-select',
    props: {
      dataSource:Array,
      indexPra:{type: Number},
    },
    data() {
      return {
       chooseIndex:0,
       positions:[]
      } 
    },
    created:function(){
        this.positions = this.dataSource;
        var self = this;
        this.positions.forEach(function(item){
      //    item.chooseCode = 0;
      //    item.hasItem = false;
          self.$set(item,'chooseCode',0);
          self.$set(item,'hasItem',false);
          if(item.selectMode === 1) item.positionArray.unshift({
            "code": 0, "name": "不限",
          });
        });
        console.log(this.positions);
      },
    methods:{
        choose:function(e,index){
        this.chooseIndex = index;
    },
    clear:function(){
        this.positions.forEach(item=>{
        item.hasItem = false;
        item.chooseCode = 0;
      });
    },
    select:function(indexP,e,item){
      console.log(indexP,e);
      var isActive = e.currentTarget.classList.contains('active');
      var fromFirst = (this.indexPra === void 0);
      if(!isActive && item.code !== 0){
          this.positions.forEach(item=>{
        item.hasItem = false;
        item.chooseCode = 0;
      });
      this.positions[indexP].hasItem = true;
      this.positions[indexP].chooseCode = item.code;
      }else{
          this.positions[indexP].hasItem = false;
          this.positions[indexP].chooseCode = 0;
      }
      if(fromFirst && isActive) return;
      if(fromFirst && this.positions[indexP].selectMode === 1){
        //父级筛选清空子级
        if(this.$refs.subFilter){
            this.$refs.subFilter.forEach(item=>item.clear());
        }
      }else{
        indexP = this.indexPra;
      }
      if(fromFirst){
        this.$emit('select',item);
      }else{

        this.$emit('select',indexP,e,item);
      }
      
    }
  }
}
</script>