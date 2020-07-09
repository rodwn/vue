<template>
  <div>
    <div id="screen" v-bind:class="state" @click="onClick">{{guide}}</div>
    <button type="button" v-on:click="initializing">초기화</button>
  </div>
</template>

<script>
const randomTimeAsMs = () => {
  return Math.floor(Math.random() * 4000) + 500;
}
export default {
  data() {
    return {
      guide: '반응속도 테스트 시작하려면 클릭',
      state: 'waiting',
      timeStamp: '',
      setTimeoutID: ''
    }
  },
  methods: {
    onClick () {
      switch(this.state) {
        case 'waiting':
          this.state = 'ready';
          this.guide = '화면이 녹색으로 바뀌면 클릭하셈';
          this.setTimeoutID = setTimeout(this.setTimeoutHandler, randomTimeAsMs());
          break;
        case 'ready':
          clearTimeout(this.setTimeoutID);
          this.guide = '너무 빨리 누르셨습니다. 재 도전하려면 다시 클릭하셈';
          this.state = 'waiting';
          break;
        case 'action':
          this.state = null;
          this.guide = `${(Date.now() - this.timeStamp) / 1000}초!`;
          break;
      }
    },
    setTimeoutHandler () {
      this.state = 'action';
      this.guide = '누르셈!';
      this.timeStamp = Date.now();
    },
    initializing () {
      clearTimeout(this.setTimeoutID);
      this.guide = '반응속도 테스트 시작하려면 클릭';
      this.state = 'waiting';
      this.timeStamp = '';
      this.setTimeoutID = '';
    }
  }
}
</script>

<style scoped>
  #screen {
    width: 80%;
    margin: 100px auto;
    padding: 100px 0;
    background-color: #ccc;
    text-align: center;
  }
  #screen.waiting {
    background-color: mint;
  }
  #screen.ready {
    background-color: red;
  }
  #screen.action {
    background-color: green;
  }
</style>