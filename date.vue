<!--日期-->
<style scoped>
  html, body {
    width: 100%;
  }

  .myDatePicker {
    width: 100%;
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    z-index: 999;
    background-color: #fff;
    overflow: auto;
  }

  .fix:after {
    content: '';
    clear: both;
    display: block;
  }

  .headWrap {
    box-sizing: border-box;
    background-color: #0094db;
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    z-index: 1;
  }

  .head {
    width: 100%;
    color: #fff;
    font-size: 0.5rem; /*px*/
    text-align: center;
    height: 1rem;
    line-height: 1rem;

  }

  .backImg {
    float: left;
    width: 1.5rem;
    text-align: center;
    height: 100%;
    background: url("./back.png") no-repeat 0.3rem;
  }

  .rightInfo {
    float: right;
    width: 1.5rem;
    text-align: left;
    height: 100%;
  }

  .centerInfo {
    text-align: center;
    width: 6.8rem;
    float: left;
  }

  .week {
    width: 100%;
    display: flex;
    background-color: #ccc;
    font-size: 0.32rem; /*px*/
    line-height: 0.8rem;
    height: 0.8rem;
  }

  .week div {
    flex: 1;
    text-align: center;
  }

  .weekend {
    color: #0094db;
  }

  .dateBox {
    font-size: 0.32rem;
    width: 100%;
    padding-top: 1.8rem;
  }

  .year, .month, .days {
    width: 100%;
  }

  .month {
    margin-bottom: 0.20rem;
    /*height: 4rem;*/
  }

  .monthTitle {
    background-color: #eee;
    text-align: center;
    width: 100%;
    line-height: 0.8rem;
    height: 0.8rem;
  }

  .days {
    width: 100%;
    /*display: flex;*/
    /*flex-direction:row;*/
    /*justify-content: space-around;*/
  }

  .day {
    font-size: 0.35rem;
    height: 0.8rem;
    line-height: 0.8rem;
    width: 1rem;
    text-align: center;
    float: left;
    margin: 0.2rem;
  }

  .dayActive {
    background-color: #0094db;
    color: #fff;
    border-radius: 50%;
    position: relative;
  }

  .dayruzhu {
    background-color: #fff;
    color: #ccc;
  }

  .dayKong {
    background-color: #fff;
  }

  .no {
    color: #ccc;
  }

  .tipDiv {
    line-height: 0.60rem;
    background-color: red !important;
  }


</style>


<!--动画-->
<style scoped>
  .fade-enter-active, .fade-leave-active {
    transition: all 0.5s ease;
  }

  .fade-enter, .fade-leave-active {
    opacity: 0;
    transform: translateY(0.300rem);
  }
</style>


<template>
  <transition name="fade">
    <div class="myDatePicker">
      <div class="headWrap">
        <div class="head fix">
          <div class="backImg" @click="dateBack"></div>
          <div class="centerInfo" v-text="centerInfo" @click="centerClick"></div>
          <div class="rightInfo" v-text="rightInfo" @click="rightClick"></div>
        </div>
        <div class="week fix">
          <div class="weekend">日</div>
          <div>一</div>
          <div>二</div>
          <div>三</div>
          <div>四</div>
          <div>五</div>
          <div class="weekend">六</div>
        </div>
      </div>
      <div class="dateBox">
        <div class="year" v-for="(item,index) in years" @click="yearClick($event,index)">
          <div class="month fix" v-for="(item,index) in years[index].dateYear" @click="monthClick($event,index)">
            <div class="monthTitle">{{item.year}}年{{item.month}}月</div>
            <div class="fix days">
              <div class="day" v-for="(itemDay,index) in item.days" @click="dayClick($event,index)"
                   :time="item.year + '-'+ item.month + '-' + itemDay.day" ref="day"
                   @touchend="dayClickEnd" :class="{no:itemDay.no}">{{itemDay.day}}
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </transition>
</template>


<script>

  var html = document.getElementsByTagName('html')[0];

  html.style.fontSize = '100px';


  var currentTime = new Date();
  var currentYear = currentTime.getFullYear();
  var currentMonth = currentTime.getMonth() + 1;
  var currentDate = currentTime.getDate();
  var currentDay = currentTime.getDay();
  var currentMinWeek;


  if (currentMonth < 10) {
    currentMonth = '0' + currentMonth
  }

  if (currentDate < 10) {
    currentDate = '0' + currentDate
  }


  export default{
    data(){
      return {
        jintian: '',//只是天
        yearIndex: 0,
        monthIndex: 0,
        dayIndex: 0,
        years: [],
        selDay: '',
        selMonth: '',
        selYear: '',
      }
    },
    props: {
      centerInfo: {default: '选择日期'},
      tipDivText: {default: ''},
      rightInfo: {default: ''},
      openTime: {default: ''},
      min: {default: currentYear + '-' + currentMonth + '-' + currentDate},//今天
      max: {default: currentYear + 1 + '-' + currentMonth + '-' + currentDate},//一年
    },
    methods: {
      num9(num){
        num = parseInt(num);
        if (num < 10) {
          num = '0' + num;
          return num;
        } else {
          return num;
        }
      },
      yearClick(e, index){
        if (e.target.className == 'day no') {
          return;
        }

        this.yearIndex = index;

        var day = this.years[this.yearIndex].dateYear[this.monthIndex].days[this.dayIndex].day;
        var month = this.years[this.yearIndex].dateYear[this.monthIndex].month;
        var year = this.years[this.yearIndex].dateYear[this.monthIndex].year;

        if (day == '今天') {
          day = this.jintian;
        }
        var selTime = year + '-' + month + '-' + day;


        if (e.target.className == 'day' || e.target.className == 'day dayActive') {
          this.$emit('dateClick', selTime);
        }
      },
      monthClick(e, index){
        this.monthIndex = index;
      },
      dayClick(e, index){
        if (e.target.className == 'day no') {
          return;
        } else {
          var my = document.querySelector('.tipDiv');
          if (my != null) my.parentNode.removeChild(my);


          var jj = document.querySelectorAll('.day');
          for (var i = 0; i < jj.length; i++) {
            if (jj[i].className == 'day no') {

            } else {
              jj[i].className = 'day'
            }
          }

          e.target.className = 'day dayActive';
          this.dayIndex = index;

          if (e.target.innerText == '') {
            e.target.className = 'day dayKong'
          }

          var tipDiv = document.createElement("div");
          tipDiv.innerText = this.tipDivText;

          e.target.appendChild(tipDiv);

          tipDiv.className = 'tipDiv';
          tipDiv.style.backgroundColor = '#0094db';
          tipDiv.style.lineHeight = '50px';
        }

        var currentDayDiv = document.querySelectorAll('.day');

        currentDayDiv[currentDate - 1 + currentMinWeek].className = 'day dayActive';


      },
      dayClickEnd(){
      },
      dateBack(){
//        this.$router.go(-1);
        this.$emit('dateBack');
      },
      centerClick(){
        this.$emit('centerClick');
      },
      rightClick(){
        this.$emit('rightClick');
      },
      up(){
        //在单选时间插件,这是要用的..感觉很奇怪.在双选的时候是不需要的  应该是数据转换的问题
        var jj = document.querySelectorAll('.day');
        for (var i = 0; i < jj.length; i++) {
          if (jj[i].className == 'day no' || jj[i].className == 'day dayActive dayJintian') {

          } else {
            jj[i].className = 'day'
          }
        }

        var my = document.querySelector('.tipDiv');
        if (my != null) my.parentNode.removeChild(my);

        var target = this.openTime;
        var jinTime = currentYear + '-' + currentMonth + '-' + '今天';
        var currentTime = currentYear + '-' + currentMonth + '-' + currentDate;
        if (this.openTime != '') {
//          this.$emit('dateClick', this.openTime);
          for (var i = 0; i < this.$refs.day.length; i++) {
            if (currentTime == target) {

              if (this.$refs.day[i].attributes.time.nodeValue == jinTime) {
                var e = this.$refs.day[i];
                e.className = 'day dayActive';
                var tipDiv = document.createElement("div");
                tipDiv.innerText = this.tipDivText;
                e.appendChild(tipDiv);

                tipDiv.className = 'tipDiv';
                tipDiv.style.backgroundColor = '#0094db';
                tipDiv.style.lineHeight = '50px';
              }

            } else if (this.$refs.day[i].attributes.time.nodeValue == target) {
              var e = this.$refs.day[i];
              e.className = 'day dayActive';
              var tipDiv = document.createElement("div");
              tipDiv.innerText = this.tipDivText;
              e.appendChild(tipDiv);

              tipDiv.className = 'tipDiv';
              tipDiv.style.backgroundColor = '#0094db';
              tipDiv.style.lineHeight = '50px';
            }


          }
        } else {
          for (var i = 0; i < this.$refs.day.length; i++) {
            if (this.$refs.day[i].attributes.time.nodeValue == jinTime) {
              var e = this.$refs.day[i];
              e.className = 'day dayActive';
              var tipDiv = document.createElement("div");
              tipDiv.innerText = this.tipDivText;
              e.appendChild(tipDiv);

              tipDiv.className = 'tipDiv';
              tipDiv.style.backgroundColor = '#0094db';
              tipDiv.style.lineHeight = '50px';
            }
          }
        }

      }
    },
    mounted(){
      var minYear = parseInt(this.min.substring(0, 4));
      var minMonth = parseInt(this.min.substring(5, 7));
      var minDay = parseInt(this.min.substring(8, 10));
      var maxYear = parseInt(this.max.substring(0, 4));
      var maxMonth = parseInt(this.max.substring(5, 7));
      var maxDay = parseInt(this.max.substring(8, 10));
      var yearLength = maxYear - minYear + 1;
      var years = [];
      for (var n = 0; n < yearLength; n++) {
        var yearJson = {
          'dateYear': [
            {
              year: minYear + n,
              month: '01',
              days: [{day: '01'}, {day: '02'}, {day: '03'}, {day: '04'}, {day: '05'}, {day: '06'}, {day: '07'}, {day: '08'}, {day: '09'}, {day: '10'}, {day: '11'}, {day: '12'}, {day: '13'}, {day: '14'}, {day: '15'}, {day: '16'}, {day: '17'}, {day: '18'}, {day: '19'}, {day: '20'}, {day: '21'}, {day: '22'}, {day: '23'}, {day: '24'}, {day: '25'}, {day: '26'}, {day: '27'}, {day: '28'}, {day: '29'}, {day: '30'}, {day: '31'}]
            },
            {
              year: minYear + n,
              month: '02',
              days: [{day: '01'}, {day: '02'}, {day: '03'}, {day: '04'}, {day: '05'}, {day: '06'}, {day: '07'}, {day: '08'}, {day: '09'}, {day: '10'}, {day: '11'}, {day: '12'}, {day: '13'}, {day: '14'}, {day: '15'}, {day: '16'}, {day: '17'}, {day: '18'}, {day: '19'}, {day: '20'}, {day: '21'}, {day: '22'}, {day: '23'}, {day: '24'}, {day: '25'}, {day: '26'}, {day: '27'}, {day: '28'},]
            },
            {
              year: minYear + n,
              month: '03',
              days: [{day: '01'}, {day: '02'}, {day: '03'}, {day: '04'}, {day: '05'}, {day: '06'}, {day: '07'}, {day: '08'}, {day: '09'}, {day: '10'}, {day: '11'}, {day: '12'}, {day: '13'}, {day: '14'}, {day: '15'}, {day: '16'}, {day: '17'}, {day: '18'}, {day: '19'}, {day: '20'}, {day: '21'}, {day: '22'}, {day: '23'}, {day: '24'}, {day: '25'}, {day: '26'}, {day: '27'}, {day: '28'}, {day: '29'}, {day: '30'}, {day: '31'}]
            },
            {
              year: minYear + n,
              month: '04',
              days: [{day: '01'}, {day: '02'}, {day: '03'}, {day: '04'}, {day: '05'}, {day: '06'}, {day: '07'}, {day: '08'}, {day: '09'}, {day: '10'}, {day: '11'}, {day: '12'}, {day: '13'}, {day: '14'}, {day: '15'}, {day: '16'}, {day: '17'}, {day: '18'}, {day: '19'}, {day: '20'}, {day: '21'}, {day: '22'}, {day: '23'}, {day: '24'}, {day: '25'}, {day: '26'}, {day: '27'}, {day: '28'}, {day: '29'}, {day: '30'},]
            },
            {
              year: minYear + n,
              month: '05',
              days: [{day: '01'}, {day: '02'}, {day: '03'}, {day: '04'}, {day: '05'}, {day: '06'}, {day: '07'}, {day: '08'}, {day: '09'}, {day: '10'}, {day: '11'}, {day: '12'}, {day: '13'}, {day: '14'}, {day: '15'}, {day: '16'}, {day: '17'}, {day: '18'}, {day: '19'}, {day: '20'}, {day: '21'}, {day: '22'}, {day: '23'}, {day: '24'}, {day: '25'}, {day: '26'}, {day: '27'}, {day: '28'}, {day: '29'}, {day: '30'}, {day: '31'}]
            },
            {
              year: minYear + n,
              month: '06',
              days: [{day: '01'}, {day: '02'}, {day: '03'}, {day: '04'}, {day: '05'}, {day: '06'}, {day: '07'}, {day: '08'}, {day: '09'}, {day: '10'}, {day: '11'}, {day: '12'}, {day: '13'}, {day: '14'}, {day: '15'}, {day: '16'}, {day: '17'}, {day: '18'}, {day: '19'}, {day: '20'}, {day: '21'}, {day: '22'}, {day: '23'}, {day: '24'}, {day: '25'}, {day: '26'}, {day: '27'}, {day: '28'}, {day: '29'}, {day: '30'},]
            },
            {
              year: minYear + n,
              month: '07',
              days: [{day: '01'}, {day: '02'}, {day: '03'}, {day: '04'}, {day: '05'}, {day: '06'}, {day: '07'}, {day: '08'}, {day: '09'}, {day: '10'}, {day: '11'}, {day: '12'}, {day: '13'}, {day: '14'}, {day: '15'}, {day: '16'}, {day: '17'}, {day: '18'}, {day: '19'}, {day: '20'}, {day: '21'}, {day: '22'}, {day: '23'}, {day: '24'}, {day: '25'}, {day: '26'}, {day: '27'}, {day: '28'}, {day: '29'}, {day: '30'}, {day: '31'}]
            },
            {
              year: minYear + n,
              month: '08',
              days: [{day: '01'}, {day: '02'}, {day: '03'}, {day: '04'}, {day: '05'}, {day: '06'}, {day: '07'}, {day: '08'}, {day: '09'}, {day: '10'}, {day: '11'}, {day: '12'}, {day: '13'}, {day: '14'}, {day: '15'}, {day: '16'}, {day: '17'}, {day: '18'}, {day: '19'}, {day: '20'}, {day: '21'}, {day: '22'}, {day: '23'}, {day: '24'}, {day: '25'}, {day: '26'}, {day: '27'}, {day: '28'}, {day: '29'}, {day: '30'}, {day: '31'}]
            },
            {
              year: minYear + n,
              month: '09',
              days: [{day: '01'}, {day: '02'}, {day: '03'}, {day: '04'}, {day: '05'}, {day: '06'}, {day: '07'}, {day: '08'}, {day: '09'}, {day: '10'}, {day: '11'}, {day: '12'}, {day: '13'}, {day: '14'}, {day: '15'}, {day: '16'}, {day: '17'}, {day: '18'}, {day: '19'}, {day: '20'}, {day: '21'}, {day: '22'}, {day: '23'}, {day: '24'}, {day: '25'}, {day: '26'}, {day: '27'}, {day: '28'}, {day: '29'}, {day: '30'},]
            },
            {
              year: minYear + n,
              month: '10',
              days: [{day: '01'}, {day: '02'}, {day: '03'}, {day: '04'}, {day: '05'}, {day: '06'}, {day: '07'}, {day: '08'}, {day: '09'}, {day: '10'}, {day: '11'}, {day: '12'}, {day: '13'}, {day: '14'}, {day: '15'}, {day: '16'}, {day: '17'}, {day: '18'}, {day: '19'}, {day: '20'}, {day: '21'}, {day: '22'}, {day: '23'}, {day: '24'}, {day: '25'}, {day: '26'}, {day: '27'}, {day: '28'}, {day: '29'}, {day: '30'}, {day: '31'}]
            },
            {
              year: minYear + n,
              month: '11',
              days: [{day: '01'}, {day: '02'}, {day: '03'}, {day: '04'}, {day: '05'}, {day: '06'}, {day: '07'}, {day: '08'}, {day: '09'}, {day: '10'}, {day: '11'}, {day: '12'}, {day: '13'}, {day: '14'}, {day: '15'}, {day: '16'}, {day: '17'}, {day: '18'}, {day: '19'}, {day: '20'}, {day: '21'}, {day: '22'}, {day: '23'}, {day: '24'}, {day: '25'}, {day: '26'}, {day: '27'}, {day: '28'}, {day: '29'}, {day: '30'},]
            },
            {
              year: minYear + n,
              month: '12',
              days: [{day: '01'}, {day: '02'}, {day: '03'}, {day: '04'}, {day: '05'}, {day: '06'}, {day: '07'}, {day: '08'}, {day: '09'}, {day: '10'}, {day: '11'}, {day: '12'}, {day: '13'}, {day: '14'}, {day: '15'}, {day: '16'}, {day: '17'}, {day: '18'}, {day: '19'}, {day: '20'}, {day: '21'}, {day: '22'}, {day: '23'}, {day: '24'}, {day: '25'}, {day: '26'}, {day: '27'}, {day: '28'}, {day: '29'}, {day: '30'}, {day: '31'}]
            }
          ],
          'year': minYear + n
        };
        years.push(yearJson);
      }
      var json29 = {day: '29'};
      for (var i = 0; i < years.length; i++) {
        var year = years[i].year;
        if ((year % 4 == 0) && (year % 100 != 0) || (year % 400 == 0)) {
          years[i].dateYear[1].days.push(json29);
        } else {
        }
      }
      var json0 = {day: '', kong: true};
      for (var i = 0; i < years.length; i++) {
        for (var j = 0; j < years[i].dateYear.length; j++) {
          var time = years[i].dateYear[j].year + '-' + years[i].dateYear[j].month + '-' + years[i].dateYear[j].days[0].day;
          var week = new Date(time).getDay();
          for (var k = 0; k < week; k++) {
            years[i].dateYear[j].days.unshift(json0);
          }

        }
      }

      if (yearLength == 1) {
        var jj = years[0].dateYear.slice(minMonth - 1, maxMonth);

        years[0].dateYear = jj;


      } else if (yearLength > 1) {
        years[0].dateYear.splice(0, minMonth - 1);
        years[yearLength - 1].dateYear.splice(maxMonth);
      }

      if (yearLength == 1) {
        maxMonth = maxMonth - minMonth + 1
      }

      var minMonthLength = years[0].dateYear[0].days.length;
      var minDayKong = 0;
      for (var i = 0; i < minMonthLength; i++) {
        if (years[0].dateYear[0].days[i].kong == true) {
          minDayKong++;
        }
      }
      var minDayCut = minDayKong + minDay;
      for (var i = 0; i < minDayCut - 1; i++) {
        years[0].dateYear[0].days[i].no = true;
      }

      var maxMonthLength = years[yearLength - 1].dateYear[maxMonth - 1].days.length;

      var maxDayKong = 0;

      for (var i = 0; i < maxMonthLength; i++) {
        if (years[yearLength - 1].dateYear[maxMonth - 1].days[i].kong == true) {
          maxDayKong++;
        }
      }
      var maxDayCut = maxDayKong + maxDay;
      for (var i = maxMonthLength - 1; i > maxDayCut - 1; i--) {
        years[yearLength - 1].dateYear[maxMonth - 1].days[i].no = true;
      }

      if (parseInt(currentMonth) == parseInt(minMonth)) {
        this.jintian = currentDate;
        var currentMonthTime = currentYear + '/' + currentMonth + '/' + '01';
        currentMinWeek = new Date(currentMonthTime).getDay();
        years[0].dateYear[0].days[parseInt(currentDate) + currentMinWeek - 1].day = '今天';
      }

      this.years = years;

    },
    computed: {},
    components: {},
    watch: {
      openTime(val, oldVal){
        if (val != oldVal) {
          this.up();
        }
      }
    },
    updated(){
      //设置模糊高度
      /* var section = document.querySelector('.myDatePicker');
       var h = document.body.clientHeight;
       section.style.height = h + 'px';*/


      /* var currentDayDiv = document.querySelectorAll('.day');

       currentDayDiv[currentDate - 1 + currentMinWeek].className = 'day dayActive dayJintian';*/

//      console.log(this.openTime);

      this.up();

    }
  }


</script>
