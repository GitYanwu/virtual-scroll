<template>
  <!-- 最外层放置一个DIV 用于 滚动区滚动 -->
  <div class="scoll-box">
    <div>
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
    let timeStart = Date.now();
    for(let i=0;i< 40;i++){
      this.msgList.push({
        name: "name" + i,
        id: i
      }); 
    }
    setTimeout(() => {
    }, 0);
  },
  mounted(){
    this.updateVisibleItems();
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
            // 计算界面上展示的 Item 数量(这里需要 +1 是因为如果计算出来的是一个小数 需要向上进行取整 代表页面上至少展示的 Item 个数 )
            const count = ~~(clientHeight / this.itemHeight) + 1

            
            this.endIndex = this.startIndex + count

            console.log(count)
            console.log(this.endIndex)

            // 截取部分显示数据
            this.scrollList = this.msgList.slice(this.startIndex, this.endIndex + 1)

            console.log(this.scrollList);
        }




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
