<template>
    <div>
      <div>{{msg}}</div>
      <div class="content">
        <div class="contentLfet"></div>
        <div class="contentRight">
          <div class="contentTop">
            <div class="myday">{{dateNum.day}}</div>
            <div class="mymonth">Dec</div>
          </div>
          <div v-show="isShowConetent">{{todayContent}}</div>
        </div>
      </div>
      <div class="bottom">
        <div class="bottomLfet">
          <div>宜</div>
          <div v-if="isShow">{{todos[0]}}</div>
          <div>{{todos[1]}}</div>
          <div>{{todos[2]}}</div>
        </div>
        <div class="bottomRight">
          <div>不宜</div>
          <div v-if="isShow">{{todos[0]}}</div>
          <div>{{todos[1]}}</div>
          <div>{{todos[2]}}</div>
        </div>
      </div>
    </div>
</template>

<script>
export default {
  name: 'calendar',
  data () {
    return {
      msg: '程序员日历',
      randomNum: [],
      randomThings: {},
      todos: [],
      todayContent: '',
      isShow: false,
      isShowConetent: false,
      dateNum: {
        year: 2018,
        month: '',
        day: ''
      }
    }
  },
  watch: {
    dateNum: 'getJsonData'
  },
  methods: {
    getRandom: function (min, max) {
      var num = Math.round(Math.random() * (max - min) + min)
      return num
    },
    getDate: function () {
      let myDate = new Date()
      this.dateNum.month = myDate.getMonth() + 1
      this.dateNum.day = myDate.getDate()
      console.log(myDate.getMonth())
      console.log(myDate.getDate())
    },
    getJsonData: function () {
      this.getDate()
      this.getRandom(0, 25)
      for (let i = 0; i < 3; i++) {
        this.randomNum.push(this.getRandom(0, 3))
      }
      this.$http.get('../../static/myData.json').then((response) => {
        this.randomThings = response.data
        this.todos[0] = this.randomThings[this.randomNum[0]].todo
        this.todos[1] = this.randomThings[this.randomNum[1]].todo
        this.todos[2] = this.randomThings[this.randomNum[2]].todo
        this.isShow = true
      })
    },
    getTodayContent: function () {
      let sort = Math.ceil((new Date() - new Date(new Date().getFullYear().toString())) / (24 * 60 * 60 * 1000)) + 1
      console.log(sort)
      this.$http.get('../../static/contentData.json').then((response) => {
        this.isShowConetent = true
        this.todayContent = response.data[sort].content
        console.log(response.data[sort].content)
      })
      return sort
    }
  },
  mounted: function () {
    this.getDate()
    this.getTodayContent()
  }
}
</script>

<style scoped>
  .content {
    width: 100%;
    background-color: aqua;
    height: 300px;
    display: flex;
  }
  .contentLfet {
    width: 40%;
    height: 300px;
    background-color: beige;
  }
  .contentRight {
    width: 60%;
    height: 300px;
    background-color: red;
  }
  .contentTop {
    width: 100%;
    height: 200px;
    background-color: white;
    display: flex;
  }
  .bottom {
    width: 100%;
    background-color: aqua;
    height: 180px;
    display: flex;
  }
  .bottomLfet {
    width: 50%;
    height: 180px;
    background-color: coral;
  }
  .bottomRight {
    width: 50%;
    height: 180px;
    background-color: pink;
  }
  .myday {
    font-size: 80px;
    margin-top: 30px;
  }
  .mymonth {
    margin-top: 90px;
  }
</style>
