# datePicker-vue

> A Vue.js 2.0  时间选择 project

首先
npm install datepicker-vue --save-dev

使用方法：
//<template><br/>
  <div class="hello"><br/>
    <div @click="dateShow = true"><br/>
      <input type="text" readonly v-model="dateTime" placeholder="请选择时间" style="font-size: 40px"/><br/>
    </div><br/>
    <date @dateClick="dateClick" @dateBack="dateBack" v-show="dateShow" :max="max" :openTime="openTime" :min="min"></date><br/>
  </div><br/>

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

//可选择绑定属性<br/>
tipDivText  //就是文字提示信息，默认为空。 可以为'出发啊，等文字提示信息'<br/>
min         //最小时间。默认今天<br/>
max         //最大时间。默认2019-05-20<br/>
openTime    //传给时间插件展示的选中时间。默认今天。<br/>

![效果图](./img/show1.png)
![效果图](./img/show2.png)

