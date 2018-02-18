<template>
  <div>
    <div>{{this.dataList}}</div>
    <Pagecomponent :total='this.totalPage'
      @pageCallbackFn='getCurrentPage'/>
  </div>
</template>
<script>
import data from '../mock/data.json'
import Pagecomponent from '../components/Pagecomponent'

export default {
  data() {
    return {
      dataList: [],
      totalPage: data.length
    }
  },
  components: {
    Pagecomponent
  },
  methods: {
    getCurrentPage(currentPage) {
      this.dataList = data[currentPage-1].name;
    }
  }
}
</script>

