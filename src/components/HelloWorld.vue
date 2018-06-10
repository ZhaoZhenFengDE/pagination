<template>
  <page :page-no="pageNo" :current="current" :pageSize="pageSize" v-on:changePageSize="changePageSize" v-on:changeCurrent="changeCurrent"></page>
</template>

<script>
import Page from './Page'
export default {
  name: 'HelloWorld',
  components: {
    Page
  },
  data () {
    return {
      dataCount: 15,
      current: 1,
      pageSize: 10
    }
  },
  methods: {
    changePageSize (size) {
      this.pageSize = size
    },
    changeCurrent (current) {
      this.current = current
    }
  },
  computed: {
    pageNo: function () {
      return Math.ceil(this.dataCount / this.pageSize)
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
