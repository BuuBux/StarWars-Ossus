<template>
  <div class='wrapper' :class="[{ flexTop: step === 1}]">
    <div class="box" v-if="open == false">
      <transition name="fade">
        <Head v-if="step === 0"/>
      </transition>
      <label for='search'>Put here some character name and we will look for it for you</label>
      <input 
          id='search'
          name="search"
          v-model="searchValue"
          @input="handleInput"
          placeholder="E.g Luke"
        />
      <section>
          <div class='results' v-for="(item, index) in results" :key="index">
            <div>
              <h3 v-if="item.name != ''"> {{ item.name}} </h3>
              <p class="birth" v-if="item.birth_year != 'unknown'"> Birth year: {{ item.birth_year}} </p>
              <p class="eyes" v-if="item.eye_color != 'unknown'"> Eye color : {{ item.eye_color}} </p>
              <p class="height" v-if="item.height != 'unknown'"> Height : {{ item.height}} cm </p>
              <button :value="index" class="more" @click="openLog"> More information </button>
            </div>
          </div>
      </section>
      </div>
      <Information 
        v-if="selectedHero != '' && open == true"
        @checkIsOpen="checkIsOpen"
        :infos="results"
        :choose="selectedHero"
        :isOpen="open"
      />
  </div>
</template>

<script>
import axios from 'axios';
import debounce from 'lodash.debounce';
import Information from '@/components/Information.vue';
import Head from '@/components/Head.vue';

const API = 'https://swapi.co/api/people/?search=';

export default {
  name: 'Search',
  components: {
    Information,
    Head,
  },
  data() {
    return{
      loading: false,
      step: 0,
      searchValue: '',
      results: [],
      selectedHero: '',
      open: false,
    }
  },
  methods: {
    checkIsOpen: function(checkIsOpen) {
      this.open = checkIsOpen
    },
    handleInput: debounce(function() {
      this.loading = true;
      axios.get(`${API}${this.searchValue}`)
        .then((response) => {
          this.results = response.data.results;
          this.loading = true;
          this.step = 1;
          this.$emit('progress', this.step);
          //console.log(this.results)
        })
        .catch((error) => {
          console.log(error);
        });
    }, 500),
    openLog: function(e) {
      this.selectedHero = e.target.value;
      this.open = !this.open;
      //console.log(this.open);
      return [this.selectedHero, this.open];
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">

  .fade-enter-active, .fade-leave-active {
    transition: opacity .7s ease;
  }

  .fade-enter, .fade-leave-to{
    opacity: 0;
  }

  .wrapper{
    background-color: rgba(#f6f6f6, 0.3);
    display:flex;
    flex-direction: column;
    align-items: flex-start;
    justify-content: center;;
    margin:0 auto;
    min-height: 100vh;
    padding:0;
    transition: 0.7s;
  }
  .wrapper.flexTop{
    padding: 15px;
    label{
      margin:0;
      color: #0a0a0a;
      font-size: 16px;
      line-height: 24px;
      letter-spacing: 0.2px;
      font-weight: 400;
    }
    input{
      color: #0a0a0a;
      border-color: #0a0a0a;
      margin: 15px auto 55px auto;
      &:focus{
        box-shadow: 0 10px 10px -8px rgba(30, 30, 30, 0.5);
      }
    }
    .results{
      text-align: center;
      margin: 15px 0;
      div{
        background: rgba(#6e8efb, 0.3);
        margin: 0 15px;
        padding: 25px 5px;
        display: flex;
        justify-content: center;
        flex-wrap: wrap;
        height: 100%;
        transition: .3s;
        &:hover{
          box-shadow: 0 0 5px 3px rgba(#a777e3, 0.3)
        }
      }
      h3{
        font-size: 24px;
        line-height: 36px;
        margin: 10px 0;
        color: #a777e3;
        width: 100%
      }
      p.birth{
        font-size: 12px;
        margin: 0 0 25px 0;
        width: 100%;
      }
      p.eyes, p.height{
        font-size: 16px;
        line-height: 18px;
        margin: 5px 0;
        width: 100%;
      }
      button{
        padding: 10px 15px;
        margin: 15px 0;
        border: 0;
        background: linear-gradient(135deg, #6e8efb, #a777e3);
        color: #fff;
        font-weight: 700;
        font-size: 16px;
        border-radius: 5px;
        &:hover{
          cursor:pointer;
        }
      }
    }
  }

  /*.search{
    display:flex;
    flex-direction: column;
    width:100%;
    padding: 0 15px;
    background: rgba(#fff, 0.3);
    position: relative;
    color: #fff;
    text-align: center;
    label{
      margin: 30px 0 0 0;
      font-size: 14px;
    }
    section{
      .row{
          display:flex;
          justify-content: center;
          flex-wrap: wrap;
          margin: 0 -15px;
        .results{
          flex: 0 0 33.333%;
          max-width: 33.333%;
          width: 100%;
          padding:0 15px;
          margin: 15px 0;
          p.birth{
            font-size: 12px;
            margin:0;
          }
          p.eyes, p.height{
            margin: 5px 0;
            font-size: 16px;
            line-height: 24px;
          }
          button{
            margin: 15px 0;
            border:0;
            padding: 10px 15px;
            border-radius: 5px;
          }
        }
      }
    }
  }
  */

  input{
    height: 50px;
    border:0;
    border-bottom: 2px solid #fff;
    background: transparent;
    max-width: 250px;
    margin: 35px auto 50px auto;
    color: #fff;
    text-align: center;
    font-weight: 700;
    font-size: 18px;
    letter-spacing: 2px;
    transition: .5s;
    z-index: 1;
    display: block;
  }

  input::placeholder{
    color: #fff;
  }
  input:focus::placeholder{
    opacity:0.6;
  }

  input:focus{
    outline: 0;
    box-shadow: 0 10px 10px -8px rgba(255,255,255, .5)
  }

  label{
    text-align: center;
    color: #fff;
    margin-top: 35px;
    width: 100%;
    display: block;
  }

  .box{
    width: 100%;
  }

  @media (max-width: 575px){
    label{
      font-size: 10px;
    }
  }

  @media (min-width: 576px){
    .wrapper{
      max-width: 540px;
      background: none;
    }
    label{
      font-size: 14px;
    }
    .box{
      max-width: 320px;
    }
    .wrapper.flexTop .box{
      max-width: 100%;
    }
    .wrapper.flexTop section {
      display: flex;
      justify-content: center;
      flex-wrap: wrap;
    }
    .wrapper.flexTop .results{
      flex:0 0 50%;
      max-width: 50%;
      width: 100%;
    }
  }

  @media (min-width: 768px){
    .wrapper{
      max-width: 720px;
    }
    .box{
      max-width: 430px;
    }
    label{
      padding: 0 55px;
      font-size: 18px;
      line-height: 22px;
    }
  }
  
    @media (min-width: 992px){
    .wrapper{
      max-width: 960px;
    }
    .box{
      max-width: 600px;
    }
    label{
      padding: 0 25px
    }
    .wrapper.flexTop {
      label{
        font-size: 22px;
        line-height: 28px;
        font-size: 700;
        margin: 55px 0 35px 0;
      }
      .results{
        flex:0 0 33.3%;
        max-width: 33.3%;
        width: 100%;
      }
    }
  }

  @media (min-width: 1200px){
    .wrapper{
      max-width: 1140px;
    }
  }

</style>
