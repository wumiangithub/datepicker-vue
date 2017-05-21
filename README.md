# datePicker-vue

> A Vue.js 2.0  时间选择 project

基本使用步骤<br/>
npm install datepicker-vue --save-dev<br/>
import date from 'datepicker-vue'<br/>
可选择绑定的四个属性<br/>
tipDivText  就是文字提示信息，默认为空。可以为'出发'啊等简短文字提示信息'  string类型<br/> 
min         最小时间。默认今天   string类型 'yyyy-mm-dd'<br/>
max         最大时间。默认为2019-05-20   string类型 'yyyy-mm-dd'<br/>
openTime    传给时间插件展示的选中时间。默认今天。 string类型 'yyyy-mm-dd'<br/>
今天是2017.05.20对于单身狗的程序员只能在家撸代码。感觉现在vue的组件比较缺。刚好有时间写了一个。希望对大家有帮助。<br/>
如果觉得有用，希望能给一颗星星。您的鼓励是我前行最大的动力。<br/>
废话不多说看效果。<br/>
![效果图](./img/show1.png)<br/>
![效果图](./img/show2.png)<br/>
详细使用方法：<br/>
```vue
<template>  
<date @dateClick="dateClick" @dateBack="dateBack" v-show="dateShow" :max="max" :openTime="openTime" :min="min"></date>
</template> 
<script>
  import date from 'datepicker-vue'
  export default {
  data () {
    return {
      dateShow:false,
      min:'2017-05-20',//最小时间  默认为当前时间  可以不绑定这个属性..一旦绑定必须为yyyy-mm-dd字符串格式
      max:"2018-09-11",//最大时间  不能比当前时间小  可以不绑定这个属性..一旦绑定必须为yyyy-mm-dd字符串格式
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



