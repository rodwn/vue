<template>
  <div>
    <h1>숫자야구</h1>
    <p>
      서로 다른 임의의 숫자 4자리를 맞추는 게임으로 숫자 4자리를 입력하면<br>
      정답숫자 4자리에 대한 힌트를 얻을 수 있습니다.<br>
      입력하신 숫자는 중복이 있으면 안됩니다.<br>
      자릿수와 숫자가 맞으면 <strong>스트라이크</strong>, 숫자는 맞지만 자릿수가 틀리면 <strong>볼</strong> 입니다.<br>
      시도는 10번입니다. 그럼 스타뜨
     </p>
    <form @submit.prevent="onSubmit">
      <input ref="input" type="number" min="0" max="9999" v-model="value" style="width:200px;" />
      <button type="submit">입력</button>
    </form>
    <p>시도: {{tries.length}}</p>
    <div>
      <p v-if="openAnswer">{{answer.join(',')}}</p>
      <button type="button" v-on:click="openingAnswer">정 궁금하면 정답 보기</button>
    </div>
    <ul>
      <li v-for="v in tries">
        <div>{{v.try}}</div>
        <div>{{v.result}}</div>
      </li>
    </ul>
  </div>
</template>

<script>
  const getNumbers = () => {
    const range = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9];
    const numberArray = [];

    for (let i = 0; i < 4; i++) {
      numberArray.push(range.splice([Math.floor(Math.random() * range.length)], 1)[0]);
    }

    return numberArray;
  };

  export default {
    data() {
      return {
        answer: getNumbers(),
        openAnswer: false,
        tries: [],
        value: ''
      }
    },
    methods: {
      onSubmit () {
        if (!(this.checkValue(this.value))) {
          this.value = '';
          this.$refs.input.focus();
          return;
        }

        if (this.value === this.answer.join('')) {
          this.tries.push({
            try: this.value,
            result: '홈런'
          });
          alert('축하합니다. 홈런입니다. 게임을 초기화 합니다.');
          this.initializing();
        } else {
          if (this.tries.length >= 9) {
            alert(`틀렸어요. 결국 게임이 끝났습니다. 정답은 ${this.answer.join(',')} (이)었습니다. 초기화 하겠습니다.`);
            this.initializing();
          } else {
            let strike = 0;
            let ball = 0;
            for (let i = 0; i < this.answer.length; i++) {
              if (parseInt(this.value[i]) === this.answer[i]) {
                strike++;
              } else if (this.value.indexOf(this.answer[i]) !== -1 ) {
                ball++;
              }
            }

            this.tries.push({
              try: this.value,
              result: (strike === 3) ? `${strike}스트라이크 ${ball}볼! 우와우 완전 아깝네요.` : (strike === 2 ? `${strike}스트라이크 ${ball}볼! 오 ~거의다 맞췄어요!` : (strike === 1 ? `${strike}스트라이크 ${ball}볼! 1개는 맞췄는데 어딜까요? ` : (ball > 0 ? `${strike}스트라이크 ${ball}볼! 분발하세요 ` : `오~ 0스트라이크 0볼! 이게 제일 좋아요 왜냐면 4자리 숫자 전부 제외하고 생각하면 되니까요!!`)))
            });
          }
        }
        
        this.value = '';
        this.$refs.input.focus();
      },
      initializing () {
        this.answer = getNumbers();
        this.tries = [];
      },
      openingAnswer () {
        this.openAnswer = !this.openAnswer;
      },
      checkValue (value) {
        if (value.length < 4) {
          alert('4자리 입력해주셈');
          return false;
        }
        if(value.split('').map(v => parseInt(v)).some((v, i, arr) => {
          const vIndex = arr.indexOf(v);
          if (vIndex === i) return false;

          return vIndex !== -1;
        })) {
          alert('중복된 숫자가 있습니다.');
          return false;
        }

        return true;
      }
    }
  }
</script>

<style>

</style>