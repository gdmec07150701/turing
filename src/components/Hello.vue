<template>
   <div class="hello">
     <p>
       我是图灵机器人,请您提问
       <input v-model="question">
     </p>
     <p>
       答案:{{ answer }}
     </p>
   </div>
</template>

<script>
import _ from 'lodash'
export default {
  data () {
    return {
      question: '',
      answer: '只有你提了问题,才会出现答案!!'
    }
  },
  watch: {
    question: function (newQuestion) {
      this.answer = '等待你停止输入...'
      this.getAnswer()
    }
  },
  methods: {
    getAnswer: _.debounce(
      function () {
        var vm = this
        if (this.question.indexOf('?') === -1) {
          vm.answer = '问题的结尾通常都是问号的哦.;-)'
          return
        }
        vm.answer = '努力思考中'
        let baseUrl = 'api/robot/index?'
        let info = encodeURI(this.question)
        console.log(info)
        let userId = '13798143242'
        let key = '8c04e2f68dbc6cf3be2033e4a30db270'
        let targetUrl = baseUrl + 'info=' + info + '&userid=' + userId + '&key=' + key
        console.log(targetUrl)
        this.$http.get(targetUrl).then((response) => {
          console.log(response)
          vm.answer = response.data.result.text
        })
        .catch(function (error) {
          vm.answer = '出错了,无法访问图灵机器人API' + error
        })
      },500)
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
