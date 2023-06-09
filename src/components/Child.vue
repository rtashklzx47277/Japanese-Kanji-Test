<template>
  <input type="radio" name="rank" value="All" v-model="rank" checked>All
  <input type="radio" name="rank" value="N5" v-model="rank">N5
  <input type="radio" name="rank" value="N4" v-model="rank">N4
  <input type="radio" name="rank" value="N3" v-model="rank">N3
  <!-- <input type="radio" name="rank" value="N2" v-model="rank">N2 -->
  <!-- <input type="radio" name="rank" value="N1" v-model="rank">N1 -->
  <br><br>
  <select v-model="type" @change="change">
    <option v-for="(value, type) in questionBank[rank]" :key="type">{{type}}</option>
  </select>
  <h1>{{question}}</h1>
  <input type="text" autofocus v-model="yourAnswer" @keyup.enter="check">
  <br>
  <p class="solution">{{solution}}</p>
  <p>上一題目：{{lastQuestion}}</p>
  <p>正確答案：{{lastAnswer}}</p>
  <p>您的答案：{{yourLastAnswer}}</p>
  <p>中文解釋：{{chineseExplain}}</p>
</template>

<script setup>
  import { reactive, ref, watch } from 'vue';
  import questionBank from './questionBank.json';

  const random = ref(0)
  const length = ref(0)
  const rank = ref('All')
  const type = ref('')
  const question = ref('')
  const yourAnswer = ref('')
  const solution = ref('')
  const lastQuestion = ref('')
  const lastAnswer = ref('')
  const yourLastAnswer = ref('')
  const chineseExplain = ref('')
  const fontColor = ref('')

  const change = () => {
    length.value = questionBank[rank.value][type.value].length
    random.value = Math.floor(Math.random() * length.value)
    question.value = questionBank[rank.value][type.value][random.value][0]
  }

  const check = () => {
    if (yourAnswer.value !== '') {
      lastAnswer.value = ''
      for (var i = 0; i < questionBank[rank.value][type.value][random.value][1].length; i++) {
        if (lastAnswer.value !== '') lastAnswer.value = lastAnswer.value + ' / '
        lastAnswer.value = lastAnswer.value + questionBank[rank.value][type.value][random.value][1][i]
      }
      lastQuestion.value = questionBank[rank.value][type.value][random.value][0]
      chineseExplain.value = questionBank[rank.value][type.value][random.value][2]
      if (questionBank[rank.value][type.value][random.value][1].includes(yourAnswer.value)) {
        fontColor.value = 'green'
        solution.value = '正解！'
      } else {
        fontColor.value = 'red'
        solution.value = '不正解！'
      }
      var temp = random.value
      while (temp === random.value) random.value = Math.floor(Math.random() * length.value)
      question.value = questionBank[rank.value][type.value][random.value][0]
      yourLastAnswer.value = yourAnswer.value
      yourAnswer.value = ''
    } else {
      fontColor.value = 'red'
      solution.value = '請輸入答案！'
    }
  }

  for (var key in questionBank[rank.value]) type.value = key
  change()

  watch(
    rank,
    (rank) => {
      for (var key in questionBank[rank]) {
        type.value = key
        break
      }
      change()
    }
  )

</script>

<style scoped>
  select {
    font-size: 20px;
    text-align: center;
  }
  input {
    font-size: 24px;
  }
  p {
    font-weight: bold;
    font-size: 20px;
  }
  .solution {
    color: v-bind('fontColor');
    font-size: 24px;
  }
</style>
