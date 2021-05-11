<template>
  <div class="pagination">
   <ul>
        <li v-if="currentPage>1"><a v-on:click="currentPage--,pageClick()">上一页</a></li>
        <li v-if="currentPage==1"><a class="pagechange">上一页</a></li>
        <!--当前页背景色为蓝色-->
        <!-- <li v-for="index in indexs"  v-bind:class="{ 'active': cur == index}">
            <a v-on:click="btnClick(index)">{{ index }}</a>
        </li> -->
        <li v-if="currentPage<totalPage"><a v-on:click="currentPage++,pageClick()">下一页</a></li>
        <li v-if="currentPage == totalPage"><a class="pagechange">下一页</a></li>
        <li><a>共<i>{{totalPage}}</i>页</a></li>
    </ul>
  </div>
</template> 
<script>
export default {
  data() {
    return {
      data: [],
      totalPage: [],//总页数
      pageSize: 5,// 每页显示数量
      dataShow: '',
      currentPage: 0, // 默认当前显示第一页
    }
  },
  created() {
    // 初始化时currentPage赋值
    this.currentPage = this.initPage
  },
  methods: {
    nextPage() {
      if (this.currentPage === this.pageNum - 1) return ;
      this.dataShow = this.totalPage[++this.currentPage];
    },
    // 上一页
    prePage() {
      if (this.currentPage === 0) return ;
      this.dataShow = this.totalPage[--this.currentPage];
    }
  },
  watch: {
    currentPage(newVal) {
      this.jumpPage = newVal
    },
    initPage(newVal) {
      if (this.currentPage !== newVal) {
        this.currentPage = newVal
      }
    },
  },
}
</script>  