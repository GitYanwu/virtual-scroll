<template>
  <!-- 最外层放置一个DIV 用于 滚动区滚动 -->
  <div class="scoll-box">
    <div>
      <li v-for="(item, index) in msgList" :key="index" class="list-item"> 
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
        msgList: [],
        startIndex: 0,
        endIndex: 0
    }
  },
  created(){
    let timeStart = Date.now();
    for(let i=0;i< 20;i++){
      this.msgList.push({
        name: "name" + i,
        id: i
      }); 
    }
    console.log(Date.now() - timeStart)
    setTimeout(() => {
      console.log(Date.now() - timeStart)
    }, 0);

    this.updateVisibleItems();
  },
  methods:{
      // 更新选中的 list
      updateVisibleItems(){
        if(this.msgList.length === 0){
            this.startIndex = this.endIndex = 0;
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
  }
</style>
