<template>
  <div>
    <ul class="page-container">
      <li :class="{'nomore': currentPage==1}"
        @click="prePage">上一页</li>
      <li v-for="(item, index) in totallist " :key="index" 
        :class="{'activePage': currentPage==item}"
        @click="pageClick(item)"  >{{item}}</li>
      <li :class="{'nomore': currentPage==totalPage}"
        @click="nextPage">下一页</li>
    </ul>
  </div>
</template>
<script>
export default {
  data() {
    return {
      currentPage: 1,
      groupCount: 5,
      startPage: 1,
      totalPage: 1
    }
  },
  props: {
    total: {
      type: Number,
      default: 1
    }
  },
  created() {
    this.$emit('pageCallbackFn', 1)
  },
  methods: {
    pageClick(currentPage) {
      if(currentPage >= this.groupCount){
        this.startPage = currentPage - 2
      }
      if(currentPage < this.groupCount){
        this.startPage = 1
      }
      if(currentPage ===1){
        this.startPage = 1
      }
      this.currentPage = currentPage
      this.$emit('pageCallbackFn', currentPage)
    },
    prePage() {
      if(--this.currentPage ===0){
        return false
      }
      this.pageClick(this.currentPage)
    },
    nextPage() {
      if(++this.currentPage > this.total) {
        return false
      }
      this.pageClick(this.currentPage)
    }
  },
  computed: {
    totallist() {
      var len = this.total,
          temp = [];
      if(len <= 10) {
        for(var i=1;i<len;i++){
          temp.push(i)
        }
      } else {
        temp.push(1);
        let pageLength = 0;
        if(this.groupCount + this.startPage > len){
          pageLength = len
        } else {
          pageLength = this.groupCount + this.startPage
        }
        if(this.currentPage >= this.groupCount){
          temp.push('...');
        }
        for(let i=this.startPage;i<pageLength;i++){
          if(i<=len-1 && i>1){
            temp.push(i)
          }
        }
        if(len-this.startPage >= this.groupCount +1){
          temp.push('...')
        }
        temp.push(len)
      }
      return temp;
    }
  }
}
</script>
<style>
.page-container:after {
  content: '.';
  display: block;
  height: 0;
  overflow: hidden;
  clear: both;
}
.page-container {
  zoom: 1
}

.page-container li {
  list-style: none;
  float: left;
  padding: 0px 8px;
  cursor: pointer;
  border: 1px solid #ccc;
  height: 28px;
  line-height: 28px;
  margin-right: 8px;
  border-radius: 5px;
}

.page-container li:last-child {
  margin-right: 0px;
}

/*当前页样式*/
.activePage {
  color: #fff !important;
  background: #1c84c6;
  border-color: #1c84c6 !important;
}

/*没有上一页和下一页时样式*/
.nomore {
  color: #b5b5b5 !important;
  cursor: not-allowed !important;
}
</style>
