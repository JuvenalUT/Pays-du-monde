<template>

    <div class="country-container"  v-if="loading">

      <router-link :to="{ name: 'Home' }">
        <button class="btn-back">
          <img src="../assets/icons/left-arrow.svg">
          Revenir en arrière
        </button>
      </router-link>
      
      <div class="country-main">

        <div class="country-main-info">

          <img class="country-flag" :src="country.flag">
          <h2  >{{ country.translations.fr }} <span>[ {{ country.alpha2Code }} ]</span> </h2>
          <div class="divider"></div>

          <p>Capitale : <span>{{ country.capital }}</span></p>
          <div class="divider"></div>
          <p>Région : <span>{{ country.region }}</span></p>
          <p>Sous région : <span>{{ country.subregion }}</span></p>
          <div class="divider"></div>

          <p>Superficie : <span :class="surfaceColor"  >{{ formatNumber(surfaceLevelColor(country.area)) }} km²<br>
          <span class="little-text">({{ surfaceText }})</span></span></p>
          
          <p>Population : <span>{{ formatNumber(country.population) }} hab.</span></p>
          <span class="little-text" :class="populationDensiteColor"><strong>{{ populationDensiteText }}</strong></span><br>
          <span class="little-text" :class="populationDensiteColor" v-if="loading">~{{ populationDensite((country.population / country.area)) }} habitants / km²</span>
          
          
          <div class="divider"></div>

          <p>Nom de domaine : <span  ><span v-for="(domain, index) in country.topLevelDomain" :key="index">{{ domain }} - </span></span></p>
          <p class="little-text">(identifiant de domaine internet)</p>

        </div>

        <div class="country-geo">

          
          <div class="country-map" id="map" ref="map">
            <l-map
              v-if="showMap"
              :zoom="zoom"
              :center="countryPosition"
              :options="mapOptions"
              style="height: 100%"
            >
            <l-tile-layer
            :url="url"
            :attribution="attribution"
            />
            </l-map>
          </div>

          <p>Pays frontaliers :<br></p>
          <span class="country-tag" v-for="(border, index) in countryBordersFormated" :key="index">{{ border }}</span>
          <span v-if="countryBorders.length <= 0">Aucun pays frontalier</span>
          <div class="divider"></div>

          <p>Langue(s) : <span  ><span v-for="(language, index) in country.languages" :key="index">{{ language.name }} - </span></span></p>
          <div class="divider"></div>

          <p>Gini : 
            <span v-if="country.gini !== null">{{ country.gini }} %</span>
            <span v-if="country.gini == null">Statistique inconnu</span>
          </p>
          <p class="little-text">(Coefficient sur l’égalité de revenus)<br>Ce coefficient représente le <strong>niveau de distribution des revenus</strong>, où 0 % signifie que les revenus sont uniformément répartis alors que 100 % correspondrait à l'accaparement par une seule personne de toute la richesse nationale)</p>
          <div class="divider"></div>
          
          <p>Devise(s) : <span  ><span v-for="(currency, index) in country.currencies" :key="index">{{ currency.name }} {{ currency.symbol}} - </span></span></p>

        </div>
        
      </div>

      <router-link :to="{ name: 'Home' }">
        <button class="btn-back">
          <img src="../assets/icons/left-arrow.svg">
          Revenir en arrière
        </button>
      </router-link>
    <Footer />

    </div>

</template>

<script>
import axios from 'axios';
import Footer from '../components/Footer';
/**LEAFLET MAP */
import L from 'leaflet';
import { LMap, LTileLayer } from 'vue2-leaflet';


export default {
  name: 'Pays',
  components: {
    Footer,
    LMap,
    LTileLayer
  },
  data () {
    return {
      countries: [],
      countryName: this.$route.params.country_name,
      country: {},
      surfaceColor: '',
      surfaceText: '',
      populationDensiteColor: '',
      populationDensiteText: '',
      loading: false,
      countryBorders: [],
      /**
       * LEAFLET DATA
       */
      zoom: 5,
      url: 'https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png',
      attribution:
        '&copy; <a href="http://osm.org/copyright">OpenStreetMap</a> contributors | Pays du monde | Jordan Lebrethon',
      showMap: true,
      mapOptions: {
        zoomSnap: 0.5
      },
    }
  },
  methods: {
    getCountryData() {
      /**
       * On parcourt la data countries
       * Si le nom du pays == countryName
       *  Alors on retourne l'objet du pays en question dans this.country
       */
      this.countries.forEach(country => {
        if (country.name == this.countryName) {
          // On retourne le pays correspondant
          this.countryBorders = country.borders
          return this.country = country
        }
      })
    },
    formatNumber(number) {
      /**
       * Fonction qui ajoute des espaces à un nombre (ex : 500000 => 500 000)
       */
        return number.toString().replace(/(\d)(?=(\d{3})+(?!\d))/g, '$1 ')
    },
    surfaceLevelColor(number) {
      /**
       * On determine la couleur du span de la superficie en fonction de son nombre
       * 1) Bleu       : 0 à 100 000 km²              Minuscule
       * 2) Vert clair : 100 000 à 400 000 km²        Petite
       * 3) Vert       : 400 000 à 800 000 km²        Moyenne
       * 4) Orange     : 800 000 à 3 000 000 km²      Grande
       * 5) Rouge      : 3 000 000 à 20 000 000 km²   Très grande
       */
      if (number > 0 && number < 100000) {
        this.surfaceText = 'Minuscule'
        this.surfaceColor = 'text-blue' 
        return number
      } else if (number > 100000 && number < 400000) {
        this.surfaceText = 'Petite'
        this.surfaceColor = 'text-light-green'
        return number
      } else if (number > 400000 && number < 800000) {
        this.surfaceText = 'Moyenne'
        this.surfaceColor = 'text-green'
        return number
      } else if (number > 800000 && number < 3000000) {
        this.surfaceText = 'Grande'
        this.surfaceColor = 'text-orange'
        return number
      } else if (number > 3000000 && number < 20000000) {
        this.surfaceText = 'Très grande'
        this.surfaceColor = 'text-red'
        return number
      }
    },
    populationDensite(number) {

      /**
       * On determine la couleur du span de la densité de population en fonction de la population et de la superficie
       * 1) Bleu       : 0 et 500 hab/km²        Très peu dense
       * 2) Vert clair : 500 et 1000 hab/km²     Peu dense
       * 3) Vert       : 1000 et 2000 hab/km²    Assez dense
       * 4) Orange     : 2000 et 4000 hab/km²    Très dense
       * 5) Rouge      : 4000 et 6000 hab/km²    Extrêmement dense
       */
      const densitePopulation =  Math.round(number)
    
        if (densitePopulation >= 0 && densitePopulation <= 50) {
          this.populationDensiteColor = 'text-blue'
          this.populationDensiteText = 'Très peu dense'
        } else if (densitePopulation >= 50 && densitePopulation <= 200) {
          this.populationDensiteColor = 'text-light-green'
          this.populationDensiteText = 'Peu dense'
        } else if (densitePopulation >= 200 && densitePopulation <= 400) {
          this.populationDensiteColor = 'text-green'
          this.populationDensiteText = 'Assez dense'
        } else if (densitePopulation >= 400 && densitePopulation <= 700) {
          this.populationDensiteColor = 'text-orange'
          this.populationDensiteText = 'Très dense'
        } else if (densitePopulation >= 700 && densitePopulation <= 30000) {
          this.populationDensiteColor = 'text-red'
          this.populationDensiteText = 'Extrêmement dense'
        }
        return densitePopulation
        },
    scrollToTop() {
      window.scrollTo(0,0);
    }
  },
  computed: {
    countryBordersFormated() {
      let countryName = []
      /**
       * On crée une variable contenant un tableau vide
       *  On parcours le tableau countries
       *    On parcours le tableau countryBorders
       *      Si country.alpha3Code est présent dans countryBorders
       *        Alors on push country.translations.fr dans la variable crée au dessus (countryName)
       *  On return ce tableau countryName
       */
      this.countries.forEach(country => {
        this.countryBorders.forEach(border => {
          if (country.alpha3Code == border) {
            countryName.push(country.translations.fr)
          }
        })
      })
      return countryName
    },
    countryPosition() {
      return L.latLng(this.country.latlng[0], this.country.latlng[1])
    }
  },
created () {
  /**
   * Faire une requête vers l'API
   * Attendre une réponse
   * Réponse => this.countries
   */
  axios.get('https://restcountries.eu/rest/v2/all')
  .then(response => {
    this.scrollToTop()
    // On sauvegarde l'objet entier dans la variable countries
    this.countries = response.data
    // On passe la variable loading à true pour autoriser l'affichage des données
    // (tant que c'est false, on n'affiche rien)
    this.loading = true
    this.getCountryData()
    })
    .catch(err => {
      console.log('Une erreur s\'est produite :')
      console.log(err)
    })
}
}
</script>

<style lang="scss">
@import '@/scss/_variables';
.v-application a {
  text-decoration: none !important;
  color: $main-text-color !important;
}
.country-container {
    max-width: 1500px;
    margin: 0 auto;
    padding: 20px;

.text-blue {
  color: #4DC0DA;
}
.text-light-green {
  color: #4DDA85;
}
.text-green {
  color: #A4DA4D;
}
.text-orange {
  color: #DAB34D;
}
.text-red {
  color: #DA4D4D;
}

    .little-text {
      font-size: .875em;
    }
    .country-tag {
      background: $main-text-color;
      font-size: .875em;
      font-weight: 400;
      padding: 5px 20px;
      border-radius: 5px;
      margin: 5px;
      display: inline-block;
      color: #FFFFFF !important;
    }
    .country-map {
      background-color: gray;
      width: 100%;
      height: 250px;
      display: inline-block;
    }
    p {
      margin: 10px 0;
      span {
        font-weight: 700;
      }
      }
    .btn-back {
      font-size: 1em;
      font-weight: 400;
      background: $card-background-gradient;
      padding: 10px 20px;
      box-shadow: $shadow;
      margin-bottom: 40px;
      img {
        margin-right: 5px;
        transition: all .3s;
      }
      &:hover {
        img {
          margin-left: 2px;
        }
      }
    }
    .country-flag {
      max-width: 280px;
      width: 100%;
      display: block;
    }
    h2 {
      margin: 40px 0 0 0;
      font-size: 1.375em;
      span {
        font-weight: 400;
      }
    }
}
.divider {
  height: 1px;
  width: 100%;
  background: $divider-color;
  margin: 20px 0;
}


@media only screen and (min-width: 750px) {

  .country-container {
    padding: 40px;
    .country-map {
      height: 500px;
    }
    .country-main {
      display: flex;
      .country-geo {
        width: 60%;
        margin-left: auto;
      }
      .country-main-info {
        width: 30%;
      }
    }
  }
  }


</style>