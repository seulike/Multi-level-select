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

.left-nav>li.active {
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
      <li class="qt-bb-x1 qt-br-x1 business-nav" v-for="(items,index) in dataSource" :class="[index == chooseIndex ? 'active':'',tmp[index] ? 'light':'']" @click="choose($event,index)">{{items.name}}
        <em></em>
      </li>
    </ul>
    <div class="right-content">
      <ul class="hide" v-for="(items,indexP) in dataSource" :class="indexP == chooseIndex ? 'active':''">
        <template v-if="items.selectMode === 1">
          <li v-for="info in items.positionArray" class="" :data-code="info.code" :class="info.code === tmp[indexP] ? 'active':''" @click="select(indexP,$event,info)">
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
export default {
  name: 'multi-level-select',
  props: {
    dataSource: Array,
    indexPra: { type: Number },
  },
  data() {
    return {
      chooseIndex: 0,
      tmp: []
    }
  },
  watch: {
    //to use watch rather than computed,using computed the view won't be updated in time
    tmp: function () {
      var len = this.dataSource.length;
      return Array(++len).join('0').split('').map(item => item = 0);
    },
  },
  methods: {
    choose: function (e, index) {
      this.chooseIndex = index;
    },
    clear: function () {
      this.tmp.forEach((item, i) => Vue.set(this.tmp, i, 0));
    },
    select: function (indexP, e, item) {
      console.log(indexP, e);
      //whether it is choosed
      var isActive = e.currentTarget.classList.contains('active');
      //whether it is from subComponent
      var fromFirst = (this.indexPra === void 0);
      //item is not choosed and don't choose 'unlimited'
      if (!isActive && item.code !== 0) {
        this.clear();
        Vue.set(this.tmp, indexP, item.code);
        console.log(this.tmp);
      } else {
        Vue.set(this.tmp, indexP, 0);
      }
      if (fromFirst && isActive) return;
      if (fromFirst && this.dataSource[indexP].selectMode === 1) {
        //to use an item in parent component need to clear the item choosed in subComponent
        if (this.$refs.subFilter) {
          this.$refs.subFilter.forEach(item => item.clear());
        }
      } else {
        indexP = this.indexPra;
      }
      if (fromFirst) {
        //expose event to user
        this.$emit('select', item);
      } else {
        //subComponent need to notify to parent 
        this.$emit('select', indexP, e, item);
      }

    }
  }
}
</script>