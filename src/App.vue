<template>
  <div id="app" :class="[{ showResult: value === 1}, '']">
    <Search @progress="getProgress" />
  </div>
</template>

<script>
import Search from './components/Search.vue'

export default {
  name: 'app',
  components: {
    Search
  },
  data() {
    return {
      value: '',
    }
  },
  methods: {
    getProgress(step){
      this.value = step;
      return this.value;
    }
  },
}
</script>

<style lang="scss">

@import url('https://fonts.googleapis.com/css?family=Montserrat:400,700&subset=latin-ext');
  *{
    margin: 0;
    padding: 0;
    font-family: 'Montserrat', sans-serif;
    box-sizing: border-box;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
  }
  #app{
    min-height: 100vh;
    transition: 0.7s;
  }
  #app:before{
    content: '';
    position: absolute;
    top:0;
    left: 0;
    width: 100%;
    height: 100%;
    background-image: url('assets/deteched.jpg');
    background-size: cover;
    background-position: center;
    background-repeat: no-repeat;
    z-index: -1;
    transition: .7s ease-out;
  }
  #app.showResult:before{
    opacity:0;
  }

  @media (max-width: 576px){
    #app{
      background-position: 60% 0%;
    }
  }

  @media (min-width: 768px){
    #app{
      background-position: 50% 0%;
    }
  }
</style>
