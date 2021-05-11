<template>
  <div class="pagination">
    <table border="2">
      <font>用户数据</font>
      <tr>
        <th v-for="(th,index) in title"
            :key="index">{{th}} </th>
      </tr>
      <tr v-for="user in userData"
          :key="user.id">
        <td>{{user.id}}</td>
        <td>{{user.userName}}</td>
        <td>{{user.sex}}</td>
        <td>{{user.address}}</td>
      </tr>
      <tr>
        <td colspan="4">
          <div class="pageChange">
            <ul class="ui">
              <li>共{{total}}条记录</li>

              <li><a href="#"
                   v-on:click="goPage(1)">首页</a></li>
              <li><a href="#"
                   v-on:click="prevPage"
                   v-bind:disabled="isFirstPage">上一页</a></li>
              <li v-for="index in pageIndex"
                  :key="index"
                  v-bind:class="{'active': currentPage == index}">
                <a v-on:click="btnClick(index)">{{index}}</a>
              </li>
              <li><a href="#"
                   v-on:click="nextPage"
                   v-bind:disabled="isLastPage">下一页</a></li>
              <li><a href="#"
                   v-on:click="goPage(totalPage)">末页</a></li>

              <li>共{{totalPage}}页</li>
            </ul>
          </div>
        </td>
      </tr>
    </table>

  </div>
</template> 
<script>
import axios from 'axios'

export default {
  data() {
    return {
      userData: [],
      title: ['序号', '姓名', '性别', '地址'],
      totalPage: [], //总页数
      total: [], //总条数
      pageSize: 5, // 每页显示数量
      paging: 1, //当前页数
      currentPage: 1, // 默认当前显示第一页
    }
  },
  created() {
    // this.getUser()
  },
  mounted() {
    this.getUser()
  },
  methods: {
    getUser() {
      axios
        .get('http://yapi.luckly-mjw.cn/mock/50/test/users?pageIndex=1')
        .then((response) => {
          console.log(response.data)
          this.userData = response.data.data.list
          this.total = response.data.data.page.itemSum
          this.totalPage = response.data.data.page.pageSum
        })
        .catch((error) => {
          console.log(error)
        })
    },
    nextPage: function () {
      if (this.currentPage < this.totalPage) {
        this.paging++

        this.currentPage++

        // this.getUser()
      }
    },
    prevPage: function () {
      if (this.currentPage > 1) {
        this.paging--
        this.currentPage--
        // this.getUser()
      }
    },
    goPage: function (page) {
      //   if (this.paging == page) {
      //     return
      //   }
      this.paging = page
      this.currentPage = page
      //   this.getUser()
    },
    btnClick: function (data) {
      if (data != this.currentPage) {
        this.currentPage = data
      }
    },
  },
  watch: {
    currentPage: function (oldValue, newValue) {
      //   console.log(arguments)
    },
  },
  computed: {
    pageIndex: function () {
      var leftPage = 1
      var rightPage = this.totalPage
      var arr = []
      if (this.totalPage >= 5) {
        if (this.currentPage >= 3 && this.cur < this.all - 1) {
          leftPage = this.currentPage - 2
          rightPage = this.currentPage + 2
        } else {
          if (this.currentPage <= 3) {
            leftPage = 1
            rightPage = 5
          } else {
            if (this.currentPage <= this.totalPage-2) {
              
              rightPage = this.currentPage + 2
              leftPage = this.currentPage - 2
            }else{
               rightPage = this.totalPage
              leftPage = 5
            }
          }
        }
      }
      while (leftPage <= rightPage) {
        arr.push(leftPage)
        leftPage++
      }
      return arr
    },
    isFirstPage: function () {
      return this.page == 1
    },
    isLastPage: function () {
      return this.page === this.totalPage
    },
  },
}
</script>  
<style >
.pagination {
  width: 500px;
  margin: 0 auto;
}
.ui {
  list-style: none;
  margin: 0;
  padding: 0;
}
.ui li {
  margin: 0;
  display: inline-block;
}

.ui li a {
  font-size: 12px;
  line-height: 50px;
  /* display: inline-block; */
}
.pageChange a {
  border: 1px solid #ddd;
  text-decoration: none;
  padding: 3px 10px;
  color: #010a11;
  cursor: pointer;
}
.pageChange .active a {
  color: #fff;
  cursor: default;
  background-color: rgb(48, 89, 136);
  border-color: rgb(48, 89, 136);
}
</style>