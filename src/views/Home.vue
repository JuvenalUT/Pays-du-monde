<template>
  <div class="home">
    <div>
    <Header :countriesName="countriesName" @countrySelected="addCountriesToData" @regionSelected="addRegionsToData"/>
    <CountryCards :countries="countries" :country="country" :region="region" />
    <Footer />
    </div>

    

  </div>
</template>

<script>
import Header from '../components/Header'
import CountryCards from '../components/CountryCards'
import Footer from '../components/Footer'
// Axios
import axios from 'axios';

export default {
  name: 'Home',
  components: {
    Header,
    CountryCards,
    Footer
  },
  data () {
    return {
      countries: [],       // Object
      countriesName: [],   // Array of countries name
      country: '',         // Selected country (input)
      region: ''           // Selected region (input)
    }
  },
  methods: {
    // On sauvegarde le nom des pays dans countriesName
    getCountriesName() {
            this.countries.forEach(country => {
                this.countriesName.push(country.translations.fr)
            })
        },
    addCountriesToData(payload) {
      this.country = payload
    },
    addRegionsToData(payload) {
      if (payload == "Afrique") {
        this.region = "Africa"
      } else if (payload == "Amérique") {
        this.region = "Americas"
      } else if (payload == "Asie") {
        this.region = "Asia"
      } else if (payload == "Océanie") {
        this.region = "Oceania"
      } else if (payload == "Europe") {
        this.region = "Europe"
      }
    }
  },
  created () {
        axios.get('https://restcountries.eu/rest/v2/all')
        .then(response => {
            // On sauvegarde l'objet entier dans la variable countries
            this.countries = response.data
            // On sauvegarde le nom des pays dans countriesName
            this.getCountriesName()
        })
        .catch(err => {
            console.log(err)
        })
  }
}
</script>