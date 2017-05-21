# datePicker-vue

> A Vue.js 2.0  时间选择 project

基本使用步骤
首先
npm install datepicker-vue --save-dev

import date from 'datepicker-vue'

//可选择绑定属性<br/>
tipDivText  //就是文字提示信息，默认为空。 可以为'出发啊，等文字提示信息'  string类型<br/> 
min         //最小时间。默认今天   string类型 'yyyy-mm-dd'<br/>
max         //最大时间。默认2019-05-20   string类型 'yyyy-mm-dd'<br/>
openTime    //传给时间插件展示的选中时间。默认今天。 string类型 'yyyy-mm-dd'<br/>

今天是2017.05.20对于单身狗的程序员只能在家撸代码。感觉现在vue的组件比较缺。刚好有时间写了一个。希望对大家有帮助。
如果觉得有用，希望能给一颗星星。您的鼓励是我前行最大的动力。

废话不多说看效果。

![效果图](./img/show1.png)
![效果图](./img/show2.png)



详细使用方法：
//"<date @dateClick="dateClick" @dateBack="dateBack" v-show="dateShow" :max="max" :openTime="openTime" :min="min"></date><br/>”


<script><br/>
  import date from 'datepicker-vue'<br/>
  export default {<br/>
  data () {<br/>
    return {<br/>
      dateShow:false,<br/>
      min:'2017-05-20',//最小时间  默认为当前时间  可以不绑定这个属性，但是一定不能为空。要为yyyy-mm-dd格式<br/>
      max:"2018-09-11",//最大时间  不能比当前时间小  要为yyyy-mm-dd格式<br/>
      dateTime:'',//选中的时间<br/>
    }<br/>
  },<br/>
  components:{<br/>
    date<br/>
  },<br/>
  methods:{<br/>
    dateBack(){//时间插件返回按钮事件<br/>
      this.dateShow = false;<br/>
    },<br/>
    dateClick(msg){//时间插件传给当前页面的时间事件<br/>
      this.dateShow = false;<br/>
      this.dateTime = msg;<br/>
    },<br/>
  },<br/>
  computed:{<br/>
    openTime(){//这是传给时间插件的展示时间。return的这个时间最好保存在vuex中。不然显示的都是今天<br/>
      return this.dateTime //this.$store.state.dateTime<br/>
    }<br/>
  }<br/>

}<br/>
</script><br/>




