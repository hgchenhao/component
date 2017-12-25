# 基于vue2.0封装的一个分页组件

> page.vue为一个pc端的分页组件,基础的分页功能都有，基本的思路是，页面是用数据来展示的，那就直接操作相关数据来改变视图
## Getting started

> import Page from './page.vue' 
从目录引入该文件,在父组件注册使用
``` 
<page :total='total' :current-index="currentIndex" :listLen='listLen' @getPage='getPage'></page>
total：总页码
currentIndex：当前页码
listLen：页面ui要显示几个列表页
getPage： page组件把每个事件的页码发送给父组件，用来向后台发送相关请求来展示内容
``` 
## about page.vue
### html 部分
``` 
 <ul class="item" v-show="arr.length">
       <li @click="start">首页</li>
       <li @click="pre"><a href="javascript:;"><<</a></li>    上一列表页
       <li @click="currentPre"><a href="javascript:;"><</a></li>     点解当前列表页上一页
       <li v-for="(item,index) in arr" :class="{active: item===num}" @click="getPage(item)">{{item}}</li>
       <li @click="currentNext"><a href="javascript:;">></a></li>    点解当前列表页下一页
       <li @click="next"><a href="javascript:;">>></a></li>    下一列表页
       <li @click="end">尾页</li>
   </ul>
```    
### 相关数据说明
 ``` data() {
    return {
      num: Number, //表示当前页码高亮
      arr: [], //页面显示的数组
      page: Number, //一页显示多少个,可以自定义,不能大于总页码
      Remainder:Number, //是否整除
      merchant:Number, //  比较总页数和page页
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
methods 里面的相关事件，思路主要是判断当前列表页的第一项和最后一项.通过循环来该变arr成员的值  
 bash
# install dependencies
npm install

# serve with hot reload at localhost:8080
npm run dev

# build for production with minification
npm run build


```

之前用ember.js写过一个类似组件，现在基于vue2.0封装一个，方便以后用于不同项目，可以拿来直接使用
小总结：之前也接触过ng4,发现这种相似框架排除过渡动画，页面展示都是通过后台发过来或者前端模拟的数据来
渲染页面，当然这只是相通的一小部分，也是这类框架基本思想，相比于ng4感觉vue2.0更加轻量，api更加简洁，社区比较广泛
而ng4是一个很完整的框架，我们能想到的和想不到的谷歌基本都帮我们想好了,涉及到的东西太多了




