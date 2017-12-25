<template>
<div class="page">
   <ul class="item" v-show="arr.length">
       <li @click="start">首页</li>
       <li @click="pre"><a href="javascript:;"><<</a></li>
       <li @click="currentPre"><a href="javascript:;"><</a></li>    
       <li v-for="(item,index) in arr" :class="{active: item===num}" @click="getPage(item)">{{item}}</li>
       <li @click="currentNext"><a href="javascript:;">></a></li>
       <li @click="next"><a href="javascript:;">>></a></li>
       <li @click="end">尾页</li>
   </ul>
</div>
</template>
<script>
export default {
  data() {
    return {
      num: Number, //表示当前页码高亮
      arr: [], //页面显示的数组
      page: Number, //一页显示多少个,可以自定义,不能大于总页码
      Remainder:Number, //是否整除
      merchant:Number, //商
    };
  },
  props: {
    //分页的总数
    total: {
      type: Number,
      default: 5
    },
    //当前页
    currentIndex: {
      type: Number,
      default: 1
    },
    //一个列表页显示多少页码
    listLen:{
      type: Number,
      default: 5
    }
  },
  mounted() {
    this.$nextTick(() => {
      this.pagination();
    });
  },
  methods: {
    //点击当前每个页码，获取当前值，把页码发给父组件向服务端发送请求
    getPage(index) {
      this.num = index;
      this.getPageNum(index);
    },
    //点击首页
    start() {
      let [firstIndex]=this.arr;
      if (firstIndex === 1) {
         this.num=1
        alert("已经是第一个列表了");
        return;
      }
      this.pagination();
      this.getPageNum(this.num);
    },
    //点击尾页
    end() {
      let lastIndex = this.total + 1; //总页码加1  
      let [first] = this.arr; //获取数组第一项
      let temp = [];   //设置一个临时数组，用来拷贝
      if (this.arr[this.arr.length - 1] === this.total) {
        alert("已经是最后一个列表页了");
        return;
      }
      if (this.Remainder === 0) {   //是否整除
        for (let i = this.page; i > 0; i--) {
          temp.push(last - i);
        }
        this.arr = temp;
        [this.num]=this.arr
      } else {
        for (let i = this.Remainder; i > 0; i--) {
          temp.push(lastIndex - i);
        }
        this.arr = temp;
        [this.num]=this.arr
      }
      this.getPageNum(this.num);
    },
    //点击当前列表页上一页
    currentPre(){
      let [firstIndex]=this.arr;
      if(firstIndex===this.num){
        return
      }
      this.num--;
      this.getPageNum(this.num)
    },
    //点击当前列表页下一页
    currentNext(){
      let lastIndex=this.arr[this.arr.length-1]
      if(lastIndex===this.num){
          return
      }
      this.num++;
       this.getPageNum(this.num)
    },
    //用来初始化Ui效果的
    pagination() {
      this.num = this.currentIndex;
      this.page = this.listLen
      this.merchant =  this.total / this.page
      this.Remainder =  this.total % this.page
      if (this.merchant <= 1) {  //总页数小于或等于page页
        for (let k = 1; k <= this.total; k++) {
          this.arr.push(k);
        }
      } else {
        let temp = []; //设置一个临时数组用来拷贝最终展示的
        for (let i = 1; i <= this.page; i++) {
          temp.push(i);
        }
        this.arr = [...temp];
      }
    },
    //点击列表页上一页
    pre() {
      if (this.arr[0] === 1) {
        alert("第一页了");
        this.num = 1;
        return;
      }
      if (this.Remainder === 0) {
        for (let i = 0; i < this.arr.length; i++) {
          this.arr[i] = this.arr[i] - this.page;
        }
          [this.num]=this.arr
      } else {
        var first = this.arr[0];
        var temp = [];
        for (var i = this.page; i > 0; i--) {
          temp.push(first - i);
        }
        this.arr = [...temp];
          [this.num]=this.arr
      }
      this.getPageNum(this.num);
    },
    //点击列表页下一页
    next() {
      var last = this.arr[this.arr.length - 1];
      if (last === this.total) {
        alert("最后一页了");
        return;
      }
      if (this.Remainder === 0) {
        for (let i = 0; i < this.arr.length; i++) {
          this.arr[i] = this.arr[i] + this.page;
        }
        this.num = this.arr[0];
      } else {
        for (let i = 0; i < this.arr.length; i++) {
          this.arr[i] = this.arr[i] + this.page;
        }
        var temp = [];
        for (var j = 0; j < this.arr.length; j++) {  //清楚大于总页码的值
          if (this.arr[j] <= this.total) {
            temp.push(this.arr[j]);
          }
        }
        this.arr = [...temp];
        [this.num]=this.arr
      }
      this.getPageNum(this.num);
    },
    //向父组件发送点击的页码，用于向后台请求数据
    getPageNum(num) {
      this.$emit("getPage", num);
    }
  },
  computed: {}
};
</script>
<style  scoped>
li {
  list-style: none;
}
a{text-decoration: none;color: #fff;display: block;width: 100%;height: 100%}
.item {
  display: flex;
  align-items: center;
  justify-content: center;
}
.item li {
  width: 50px;
  height: 50px;
  background: salmon;
  margin: 10px;
  line-height: 50px;
  cursor: pointer;
}
.item .active {
  background: saddlebrown;
}
</style>