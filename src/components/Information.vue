<template>
    <div v-if="isOpen" class="infoPop"> 
      <span @click="isActive" class="closeTag"> 
        <svg class="svg-icon" viewBox="0 0 20 20">
					<path fill="none" d="M15.898,4.045c-0.271-0.272-0.713-0.272-0.986,0l-4.71,4.711L5.493,4.045c-0.272-0.272-0.714-0.272-0.986,0s-0.272,0.714,0,0.986l4.709,4.711l-4.71,4.711c-0.272,0.271-0.272,0.713,0,0.986c0.136,0.136,0.314,0.203,0.492,0.203c0.179,0,0.357-0.067,0.493-0.203l4.711-4.711l4.71,4.711c0.137,0.136,0.314,0.203,0.494,0.203c0.178,0,0.355-0.067,0.492-0.203c0.273-0.273,0.273-0.715,0-0.986l-4.711-4.711l4.711-4.711C16.172,4.759,16.172,4.317,15.898,4.045z"></path>
				</svg> 
      </span>
      <div class="person-info">
        <h2> {{infos[choose].name}} </h2>
        <p v-if="infos[choose].birth_year != '' && infos[choose].birth_year != 'unknown'"> Date of birth : <strong>{{infos[choose].birth_year}}</strong> </p>
        <p v-if="infos[choose].eye_color != '' && infos[choose].eye_color != 'unknown'"> Eye color : <strong>{{infos[choose].eye_color}}</strong> </p>
        <p v-if="infos[choose].gender != '' && infos[choose].gender != 'unknown'"> Gender : <strong>{{infos[choose].gender}}</strong> </p>
        <p v-if="infos[choose].hair_color != '' && infos[choose].hair_color != 'unknown' && infos[choose].hair_color != 'n/a'"> Hair color : <strong>{{infos[choose].hair_color}}</strong> </p>
        <p v-if="infos[choose].height != '' && infos[choose].height != 'unknown'"> Height : <strong>{{infos[choose].height}} cm</strong> </p>
        <p v-if="infos[choose].mass != '' && infos[choose].mass != 'unknown'"> Weight <strong>{{infos[choose].mass}} kg</strong> </p>
        <p v-if="infos[choose].skin_color != '' && infos[choose].skin_color != 'unknown'"> Skin color : <strong>{{infos[choose].skin_color}}</strong> </p>
      </div>

      <button v-if="films == ''" @click="getFilms"> Show films  </button>
      <section class="films" v-else>
        <div class="header">
          <h3> Films </h3>
          <hr />
        </div>
        <figure v-for="(film, index) in films" :key="index">
          <img :src="require(`../assets/${film.episode_id}.jpg`)" /> 
          <figcaption>
            <h3>{{film.title}} - ep. {{film.episode_id}}</h3>
            <span>{{film.release_date}}</span>
            <p>{{film.opening_crawl}}</p>
            <div class="who">
              <h4> Created by: </h4>
              <section>
                <h5> Director </h5>
                <p>{{film.director}}</p>
              </section>
              <section>
                <h5> Producer </h5>
                <p>{{film.producer}}</p>
              </section>
            </div>
          </figcaption>
        </figure>
      </section>

      <button @click="getPlanets" v-if="infos[choose].homeworld != '' && planets == ''"> Show homeworld </button>
      <section class="homeworld" v-else>
        <h3> Homeworld </h3>
        <hr />
        <p>Name : {{planets[0].name}}</p>
        <p>Climate : {{planets[0].climate}}</p>
        <p>Terrain : {{planets[0].terrain}}</p>
        <p v-if="planets[0].population != 'unknown'">Population : ~{{planets[0].population}}</p>
      </section>

      <button @click="getSpecies" v-if="infos[choose].species != '' && species == ''"> Show species </button>
      <section class="species" v-else>
        <h3> Species </h3>
        <hr />
        <p>Species: {{species[0].name}}</p>
        <p v-if="species[0].average_height != '' && species[0].average_height !='unknown'">Avg. height: {{species[0].average_height}} cm</p>
        <p v-if="species[0].average_lifespan != '' && species[0].average_lifespan !='unknown'">Avg. lifespan: {{species[0].average_lifespan}}</p>
        <p v-if="species[0].language != '' && species[0].language !='unknown'">Languag: {{species[0].language}}</p>
      </section>

      <button @click="getStarships" v-if="infos[choose].starships.length != 0 && starships.length == 0"> Show starships </button>
      <section class="starships" v-if="starships.length != 0">
        <div class="header">
          <h3> Starships </h3>
          <hr />
        </div>
        <div v-for="(ship, index) in starships" :key="index">
          <h4>Starship name <br /> {{ship.name}}</h4>
          <p>Starship model: {{ship.model}}</p>
          <p>Manufacturer: {{ship.manufacturer}}</p>
          <p>Cost: {{ship.cost_in_credits}} credits</p>
          <p>Max atmosphering speed: {{ship.max_atmosphering_speed}} km/h</p>
          <p>Crew: {{ship.crew}}</p>
          <p>Starship class: {{ship.starship_class}}</p>
          <p>Starship length: {{ship.length}} m</p>
        </div>
      </section>

      <button @click="getVehicles" v-if="infos[choose].vehicles.length != '0' && vehicles.length == '0'"> Show vehicles </button>
      <section v-if="vehicles.length != 0" class="vehicles">
        <div class="header">
          <h3> Vehicles </h3>
          <hr />
        </div>
        <div v-for="(vehicle, index) in vehicles" :key='index'>
          <h4>Vehicle name <br /> {{vehicle.name}}</h4>
          <p>Vehicle model: {{vehicle.model}}</p>
          <p>Manufacturer: {{vehicle.manufacturer}}</p>
          <p>Cost: {{vehicle.cost_in_credits}} credits</p>
          <p>Max atmosphering speed: {{vehicle.max_atmosphering_speed}} km/h</p>
          <p>Crew: {{vehicle.crew}}</p>
          <p>Cargo capacity: {{vehicle.cargo_capacity}}</p>
          <p>Vehicle length: {{vehicle.length}} m</p>
        </div>
      </section>
    
    </div>
</template>

<script>

import axios from 'axios';

export default {
  props: ['infos', 'choose', 'isOpen'],
  name: 'Information',
  data(){
    return{
      films: [],
      planets: [],
      species: [],
      starships: [],
      vehicles: [],
      used: 0,
      openIt: '',
    }
  },
  methods: {
    getFilms() {
      for(let i = 0; i<this.infos[this.choose].films.length; i++){
        axios.get(this.infos[this.choose].films[i])
        .then((response) => {
          this.films.push(response.data);
          //console.log(this.films[i])
          return [this.films.sort(this.compare)];
        })
        .catch((error) => {
          console.log(error);
        });
      }
    },
    getPlanets() {
      axios.get(this.infos[this.choose].homeworld)
      .then((response) => {
        this.planets.push(response.data);
        return [this.planets];
      })
      .catch((error) => {
        console.log(error);
      });
    },
    getSpecies() {
      axios.get(this.infos[this.choose].species)
      .then((response) => {
        this.species.push(response.data);
        return [this.species];
      })
      .catch((error) => {
        console.log(error);
      });
    },
    getStarships() {
      for(let i = 0; i<this.infos[this.choose].starships.length; i++){
        axios.get(this.infos[this.choose].starships[i])
        .then((response) => {
          this.starships.push(response.data);
          //console.table(this.starships)
          return [this.starships];
        })
        .catch((error) => {
          console.log(error);
        });
      }
    },
    getVehicles() {
      for(let i = 0; i<this.infos[this.choose].vehicles.length; i++){
        axios.get(this.infos[this.choose].vehicles[i])
        .then((response) => {
          this.vehicles.push(response.data);
          //console.log(this.vehicles);
          return [this.vehicles];
        })
        .catch((error) => {
          console.log(error);
        });
      }
    },
    isActive() {
      this.openIt = this.isOpen;
      this.openIt = !this.openIt;
      this.$emit("checkIsOpen", this.openIt) ;
      this.films = [];
      this.planets = [];
      this.species = [];
      this.starships = [];
      this.vehicles = [];
      return [this.openIt, this.films, this.planets, this.species, this.starships, this.vehicles]
    },
    compare(a, b) {
      if (a.episode_id < b.episode_id) { return -1; } else if (a.episode_id > b.episode_id) { return 1; }
        return 0;
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
  .infoPop{
    position: absolute;
    top: 0;
    left: 0;
    background: #f6f6f6;
    width:100%;
    min-height: 100vh;
    display:flex;
    justify-content: center;
    flex-wrap: wrap;
    align-items: center;
    padding: 35px;
    text-align: center;
    .person-info{
      display: flex;
      justify-content: flex-start;
      flex-wrap: wrap;
    }
    .closeTag{
      position: absolute;
      top:5px;
      right:5px;
      overflow: hidden;
      &:hover{
        cursor: pointer;
        svg{
          transform: rotate(360deg) skewX(180deg);
        }
      }
      svg{
        width: 36px;
        height: 36px;
        transition: .7s;
        path{
          fill: #0a0a0a;
        }
      }
    }
    h2, p{
      flex:0 0 100%;
      width: 100%;
      max-width: 100%;
    }
    h2{
      margin: 35px 0;
    }
    p{
      margin: 10px 0;
    }
    button{
      padding: 10px 15px;
      margin: 15px 25px;
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
    section{
      display:block;
      width: 100%;
      margin: 25px 0;
      h3{
        text-transform: uppercase;
        margin: 25px 0 10px 0;;
      }
      h4{
        margin: 45px 0 0 0;
        font-size: 24px;
        line-height: 32px;
      }
      hr{
        max-width: 420px;
        margin: 0 auto 15px auto;
      }
      >p{
        flex: 0 0 100%;
        max-width: 100%;
        width: 100%;
      }
      div{
        >p{
          flex: 0 0 100%;
          max-width: 100%;
          width: 100%;
        }
      }
      figure{
        margin: 35px 0;
        img{
          display:block;
          max-width:100%;
          height: auto;
          margin:0 auto;
        }
        h3{
          font-size: 18px;
          line-height: 24px;
          margin: 15px 0 0 0;
        }
        span{
          font-size:12px;
          line-height: 16px;
          font-weight: 700;
        }
        figcaption{
          >p{
            flex: 0 0 100%;
            max-width: 100%;
            width: 100%;
            margin: 15px 0;
            text-align: justify
          }
        }
        .who{
          display:flex;
          justify-content: flex-start;
          flex-wrap: wrap;
          h4{
            font-size: 16px;
            line-height: 24px;
            flex: 0 0 100%;
            width: 100%;
            max-width: 100%;
          }
          section{
            flex: 0 0 50%;
            max-width: 50%;
            width: 100%;
            margin: 15px 0;
            p{
              text-align: center;
            }
          }
        }
      }
    }
  }
  @media (min-width: 471px){
    .infoPop{
      button{
        margin: 20px 55px;
      }
    }
  }
  @media (min-width: 576px){
    .infoPop
    {
      justify-content: space-evenly;
      p{
        flex: 0 0 50%;
        max-width: 50%;
        width: 100%;
        &:last-child{
          flex: 0 0 100%;
          max-width: 100%;
          width: 100%;
        }
      }
      button{
        flex: 0 0 40%;
        max-width: 40%;
        width: 100%;
        margin:0 15px; 
        &:last-child{
          flex:0 0 100%;
          max-width: 100%;
          width: 100%;
        }
      }
    }
  }
  @media (min-width: 670px){
    .infoPop{
      button{
        flex: 0 0 30%;
        max-width: 30%;
        width: 100%;
        margin: 20px 10px;
        &:last-child{
          flex: 0 0 30%;
          max-width: 30%;
          width: 100%;
        }
      }
    }
  }
  @media (min-width: 991px){
    .infoPop{
      h2{
        font-size: 72px;
        line-height: 92px;
      }
      .person-info{
        p{
          font-size: 18px;
          line-height: 24px;
          flex: 0 0 33%;
          max-width: 33%;
          width: 100%;
        }
      }
      .homeworld, .species{
        flex: 0 0 50%;
        max-width: 50%;
        width: 100%;
      }
      button{
        font-size: 18px;
        line-height: 24px;
      }
      .starships, .vehicles{
        display: flex;
        justify-content: flex-start;
        flex-wrap: wrap;
        .header{
          flex: 0 0 100%;
          max-width: 100%;
          width: 100%;
        }
        div{
          flex:0 0 50%;
          max-width: 50%;
          width: 100%;
        }
      }
      .films{
        display:flex;
        justify-content: flex-start;
        flex-wrap: wrap;
        .header{flex:0 0 100%; max-width: 100%; width: 100%;}
        figure{
          flex: 0 0 50%;
          max-width: 50%;
          width: 100%;
          >*{margin-left: 25px;margin-right: 25px;}
          img{max-height: 475px; margin: 0 auto;}
          .who{
            h4{font-size: 28px;line-height: 36px;margin: 10px 0;}
            h5{font-size: 22px}
            p{font-size: 18px;}
          }
        }
      }
    }   
  }
</style>
