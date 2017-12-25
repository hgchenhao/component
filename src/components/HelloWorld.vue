<template>
  <div class="hello">
    <page :total='total' @getPage='getPage' :current-index="currentIndex" :listLen='listLen'></page>
  </div>
</template>
<script>
import Page from './page.vue'
export default {
  name: 'HelloWorld',
  data () {
    return {
      total:13,
      currentIndex:3,
      listLen:5
    }
  },
  created () {
  },
  components: {
    Page
  },
  methods: {
    getPage(index){
      //获取当前页面
      console.log(index)
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
