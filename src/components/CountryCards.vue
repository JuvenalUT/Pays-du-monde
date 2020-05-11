<template>
    <div class="cards-container">
  <div class="card" v-for="(country, index) in filterByNameOfCountry(countries)" :key="index">
    <router-link :to="{ name: 'Pays', params: { country_name: country.name } }">
    <div class="card-content">
      <div class="card-gradient-line"></div>
      <h1 class="card-title">{{ country.translations.fr }}</h1>
      <img :src="country.flag" alt="Drapeau français">
      <p class="card-title-big">{{ formatNumber(country.population) }}</p>
      <span class="card-title">Population</span>
    </div>
    </router-link>
  </div>


      
      <button class="btn-more" @click="showMoreCountries(5)" v-if="!country">Plus de pays</button>
      
    </div>
</template>

<script>

export default {
    name: 'CountryCards',
    props: ['countries', 'country', 'region'], // API Data (Object) (Parent : Home)
    data () {
        return {
          numberOfCountriesToShow: 15
        }
    },
    methods: {
      filterByNameOfCountry(countriesArray) {
        /** Si un pays est séléctionné (country == 'France')
        *   Alors on retourne le tableau des pays filtré
        *   avec le pays séléctionné, puis on affiche seulement celui-ci
        * 
        * Sinon si une région est séléctionné (region == 'Europe')
        *   Alors on retourne le tableau des pays filtré
        *   avec la région séléctionné, puis on affiche seulement ceux-là
        *
        * Sinon
        *   On retourne le tableau des pays en entier en allant de 0 jusqu'au nombre de pays à montrer (varialbe numberOfCountriesToShow) 
        */
        if (!this.country == '') {
          return countriesArray.filter(country => {
              if(country.translations.fr == this.country) {
                return country.translations.fr
              }
            }).slice(0, this.numberOfCountriesToShow)
        } else if (!this.region == '') {
          return countriesArray.filter(country => {
              if(country.region == this.region) {
                return country.translations.fr
              }
          }).slice(0, this.numberOfCountriesToShow)
        } else {
          return countriesArray.slice(0, this.numberOfCountriesToShow)
        }
            
        },
        showMoreCountries(number) {
          this.numberOfCountriesToShow = this.numberOfCountriesToShow + number
        },
        formatNumber(number) {
          return number.toString().replace(/(\d)(?=(\d{3})+(?!\d))/g, '$1 ')
        }
      },
    }

</script>

<style lang="scss" scoped>
@import '@/scss/_variables';
.cards-container {
  max-width: 1500px;
  margin: 40px auto 0 auto;
  text-align: center;
  padding-bottom: 20px;
  .btn-more {
    padding: 20px 40px;
    width: calc(100% - 80px);
    margin: 0 20px;
    border-radius: 5px;
    font-size: .875em;
    font-weight: $regular;
    color: $input-text-color;
    text-transform: uppercase;
    letter-spacing: .2em;
    line-height: 17px;
    transition: .3s all;
    &:hover {
      background-color: rgb(231, 231, 231);
    }
  }
  .card {
    animation: .3s cardFade;
    background: $card-background-gradient;
    border-radius: 4px 4px 10px 10px;
    margin: 0 20px 20px 20px;
    box-shadow: 0px 0px 20px rgba(0, 0, 0, 0.1);
    transition: .3s transform;
    &:hover {
      transform: scale3d(1.05, 1.05, 1.05);
    }
    @keyframes cardFade {
      from {
        opacity: 0;
        transform: scale3d(0, 0, 0);
      }
      to {
        opacity: 100;
        transform: scale3d(1, 1, 1);
      }
    }
    .card-content {
      display: flex;
      flex-direction: column;
      padding: 20px 30px;
      align-items: center;
      position: relative;
      .card-gradient-line {
        position: absolute;
        background: $card-gradient-line;
        height: 4px;
        width: 100%;
        top: 0;
        border-radius: 10px 10px 0 0;
      }
      img {
        display: block;
        max-width: 145px;
        width: 100%;
        height: auto;
        height: 95px;
      }
      .card-title {
        font-size: .875em;
        font-weight: $regular;
        color: $input-text-color;
        text-transform: uppercase;
        letter-spacing: .2em;
        line-height: 17px;
        margin: 12px 0;
      }
      .card-title-big {
        font-size: 1.5em;
        font-weight: $bold;
        color: $main-text-color;
        text-transform: uppercase;
        line-height: 44px;
        margin: 15px 0 0 0;
      }
    }
  }
}
@media only screen and (min-width: 380px) {
.cards-container {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;

  .card {
    width: 330px;
    margin: 20px 20px 20px 20px;
  }
}
}
</style>