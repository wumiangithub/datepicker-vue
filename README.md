# datePicker-vue

> A Vue.js 2.0  时间选择 project

首先
npm install datepicker-vue --save-dev

使用方法：
<template>
  <div class="hello">
    <div @click="dateShow = true">
      <input type="text" readonly v-model="dateTime" placeholder="请选择时间" style="font-size: 40px"/>
    </div>
    <date @dateClick="dateClick" @dateBack="dateBack" v-show="dateShow" :max="max" :openTime="openTime" :min="min"></date>
  </div>
</template>

<script>
  import date from 'datepicker-vue'
export default {
  data () {
    return {
      dateShow:false,
      min:'2017-05-20',//最小时间  默认为当前时间  可以不绑定这个属性，但是一定不能为空。要为yyyy-mm-dd格式
      max:"2018-09-11",//最大时间  不能比当前时间小  要为yyyy-mm-dd格式
      dateTime:'',//选中的时间
    }
  },
  components:{
    date
  },
  methods:{
    dateBack(){//时间插件返回按钮事件
      this.dateShow = false;
    },
    dateClick(msg){//时间插件传给当前页面的时间事件
      this.dateShow = false;
      this.dateTime = msg;
    },
  },
  computed:{
    openTime(){//这是传给时间插件的展示时间。return的这个时间最好保存在vuex中。不然显示的都是今天
      return this.dateTime //this.$store.state.dateTime
    }
  }

}
</script>

//可选择绑定属性
tipDivText  //就是文字提示信息，默认为空。 可以为'出发啊，等文字提示信息'
min         //最小时间。默认今天
max         //最大时间。默认2019-05-20
openTime    //传给时间插件展示的选中时间。默认今天。

![效果图](./img/show1.png)
![效果图](./img/show2.png)

