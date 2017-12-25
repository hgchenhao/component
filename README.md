### 基于vue2.0封装的一个分页组件

> 文件page.vue为一个pc端的分页组件,基础的分页功能都有，基本的思路是，页面是用数据来展示的，那就直接操作相关数据来改变视图
### Getting started

> import Page from './page.vue' 
从目录引入该文件,在父组件注册使用
``` 
<page :total='total' :current-index="currentIndex" :listLen='listLen' @getPage='getPage'></page>
total：总页码
currentIndex：当前页码
listLen：页面ui要显示几个列表页
getPage： page组件把每个事件的页码发送给父组件，用来向后台发送相关请求来展示内容
``` 
### about page.vue
#### html 部分
``` 
 <ul class="item" v-show="arr.length">
       <li @click="start">首页</li>
       <li @click="pre"><a href="javascript:;"><<</a></li>    上一列表页
       <li @click="currentPre"><a href="javascript:;"><</a></li>     点击当前列表页上一页
       <li v-for="(item,index) in arr" :class="{active: item===num}" @click="getPage(item)">{{item}}</li>
       <li @click="currentNext"><a href="javascript:;">></a></li>    点击当前列表页下一页
       <li @click="next"><a href="javascript:;">>></a></li>    下一列表页
       <li @click="end">尾页</li>
   </ul>
```    
#### 相关数据说明
 ``` 
 data() {
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
相关片段:
    //点击当前每个页码，获取当前值，把页码发给父组件向服务端发送请求
    getPage(index) {
      this.num = index;
      this.getPageNum(index);
    },
    //点击首页
    start() {
      let [firstIndex]=this.arr;
      if (firstIndex === 1) {
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

```
```
 bash
# install dependencies
npm install

# serve with hot reload at localhost:8080
npm run dev

# build for production with minification
npm run build


```

之前用ember.js写过一个类似组件，现在基于vue2.0封装一个，方便以后用于不同项目，可以拿来直接使用.<br>
小总结：之前也接触过ng4,发现这些相似框架排除过渡动画，页面展示都是通过后台发过来或者前端模拟的数据来
渲染页面，当然这只是相通的一小部分，也是这类框架基本思想。<br>





