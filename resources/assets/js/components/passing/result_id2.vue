<template>
  <div class="result">
    <div class="result__header h3 mb10">
      Ошибки в тесте:
    </div>
    <div class="result__header h5 mb5">
      Вопросы:
    </div>
    <div class="result__questions">
      <div class="result__question" 
        :class="{'result__question--bad': hasBad(item.answers)==0,
          'result__question--half': hasBad(item.answers)==1,
          'result__question--good': hasBad(item.answers)==2,
          'result__question--current': i==currentIndex}" 
        :title="getQuestionTitle(hasBad(item.answers))"
        v-for="(item,i) in questionsAnswers"
        @click="selectQuestion(i)">
        {{index(i)}}
      </div>
    </div>
    <div class="result__current-item">
      <div class="result__current-item-question h4 mb20">
        <div class="result__number">{{index(currentIndex)}}</div>
        <div class="ql-container" v-html="questionsAnswers[currentIndex].question.question"></div>
      </div>
      <div class="result__answers">
        <div class="result__answer" 
          :class="{
            'result__answer--right': answer.right==2,
            'result__answer--bad': answer.right==1,
            'result__answer--need': answer.right==0,
            }"
          :title="getAnswerTitle(answer.right)"
          v-for="answer in questionsAnswers[currentIndex].answers">
          {{answer.answer}}
        </div>
      </div>
    </div>
    

  </div>
</template>
<script>
  export default({
    props: ['qa'],
    data(){
      return {
        questionsAnswers: JSON.parse(this.qa),
        currentQuestion: null,
        currentIndex: null,
      }
    },
    created(){ 
      this.currentIndex= 0;

    },
    methods: {
      index(val){
        return parseInt(val)+1;
      },
      getQuestionTitle(i){
        if (i==0) return "На данный вопрос не было дано правильных ответов";
        else if (i==1) return "На данный вопрос были даны как правильные, так и неправильные ответы";
        else if (i==2) return "На данный вопрос даны все правильные ответы";
      },
      getAnswerTitle(i){
        if (i==0) return "Требовалось поставить";
        else if (i==1) return "Неправильный ответ";
        else if (i==2) return "Правильный ответ";
      },
      selectQuestion(i){
        this.currentIndex=i;
      },
      hasBad(answers){
        let bad=false;
        let good=false;
        for (let key in answers){
          if (answers[key].right<2) {
            bad=true;
          } else{
            good=true;
          }
          //if (good&&bad) break;
        }
        if (good&&!bad) return 2;
        else if (!good && bad) return 0;
        else if (good && bad) return 1;
      }
    },

  });
</script>