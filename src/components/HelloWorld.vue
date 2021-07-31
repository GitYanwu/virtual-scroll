<template>
  <!-- 最外层放置一个DIV 用于 滚动区滚动 -->
  <div class="scoll-box"  @scroll.passive="fnScroll">
    <div :style="{'paddingTop': paddingTop, 'paddingBottom': paddingBottom }">
      <li v-for="(item, index) in scrollList" :key="index" class="list-item"> 
        {{ item.name }}
      </li>
    </div>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  data () {
    return {
        // 源数组
        msgList: [],
        // 显示区域第一个元素的下标
        startIndex: 0,
        // 显示区域最后一个元素的下标
        endIndex: 0,
        // 单个 item 的高度
        itemHeight: 50,
        // 可视区域数组
        scrollList: []
    }
  },
  created(){
    for(let i=0;i< 400000;i++){
      this.msgList.push({
        name: "name" + i,
        id: i
      }); 
    }
  },
  mounted(){
    this.updateVisibleItems();
  },
  computed: {
    // 计算paddind值,用于计算滚动条
    paddingTop(){
      return this.startIndex * this.itemHeight + "px";
    },
    paddingBottom(){
      console.log(this.endIndex);
      return (this.msgList.length - this.endIndex) * this.itemHeight + "px";
    }
  },
  methods:{
      // 更新选中的 list
      updateVisibleItems(){
        if(this.msgList.length === 0){
            this.startIndex = this.endIndex = 0;
        }else{
            // 可视区域的高度
            const clientHeight = this.$el.clientHeight;
            // 滚动区滚动的距离
            const scrollTop = this.$el.scrollTop;
            // 可视区域能展示的数量(需要向上取整)
            const count = Math.ceil(clientHeight / this.itemHeight)

            // 这里 +1 是为了解决 如果在滚动过程中 第一个展示不全 避免尾部空白
            this.endIndex = (this.startIndex + count  + 1) + 10

            // 保证数组不会越界
            this.endIndex = this.endIndex > this.msgList.length ?  this.msgList.length : this.endIndex

            // 截取部分显示数据
            this.scrollList = this.msgList.slice(this.startIndex, this.endIndex)
        }
      },
      fnScroll(){
        // 滚动区滚动的时候执行
        const scrollTop = this.$el.scrollTop;
        // 向下取整 代表上部滚动区不可见个数
        const count = ~~(scrollTop / this.itemHeight)
        if(count <= 5){
          this.startIndex = 0
        }else{
          // 超过 代表头部预渲染数量已经超过了预设数量
          const topCount = count - 5;// 头部多渲染数量
          // 如果相等 代表滚动距离不足一个item的高度 不用重新计算高度
          if(topCount === this.startIndex){
            return;
          }
          this.startIndex = topCount
        }
        this.updateVisibleItems()
      }
  }
}
</script>
<style>
  
  .scoll-box{
    width: 100%;
    height: 100%;
    overflow-y: scroll;
  }

  .list-item{
      width: 100%;
      height: 50px;
      display: block;
      text-align: left;
      border-bottom: 1px solid #CCC;
      line-height: 50px;
      box-sizing: border-box;
  }
</style>
