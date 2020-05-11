<template>
<header>
    <div class="header-top">
        <img src="../assets/logo.svg" alt="Logo Pays du Monde" class="logo">
        <p>Population totale : <span>7,8 Md</span></p>
        <div class="header-inputs">
            <div id="select-country">
                <v-autocomplete
                :items="countriesName"
                class="mt-7 mb-2"
                flat
                hide-no-data
                hide-details
                placeholder="Chercher un pays... "
                solo
                color="#474E6C"
                background-color="#FFFFFF"
                prepend-inner-icon="mdi-magnify"
                height="50px"
                v-model="country"
                @change="countrySelected(country)"
                ></v-autocomplete>
            </div>
            <div id="select-region">
                <v-autocomplete
                class="mt-3"
                flat
                hide-no-data
                hide-details
                placeholder="Filtrer par région"
                solo
                color="#474E6C"
                background-color="#FFFFFF"
                height="50px"
                :items="regions"
                v-model="region"
                @change="regionSelected(region)"
                ></v-autocomplete>
            </div>
        </div>
    </div>

</header>
</template>

<script>
import { VAutocomplete } from 'vuetify/lib'


export default {
    name: 'Header',
    props: ['countriesName'], // API Data (Object) (Parent : Home)
    components: {
      VAutocomplete
    },
    data () {
        return {
            country: '',      // Selected country
            region: '',
            regions: ['Afrique', 'Amérique', 'Asie', 'Europe', 'Océanie'] // Selected regions
        }
    },
    methods: {
        countrySelected(country) {
            this.$emit('countrySelected', country)
        },
        regionSelected(region) {
            this.$emit('regionSelected', region)
        }
    }
}
</script>

<style lang="scss">
@import '@/scss/_variables';
header {
.header-top {
    max-width: 1500px;
    margin: 40px auto;
    padding: 0 20px;
    .logo {
        max-height: 50px;
    }
    .header-inputs {
        #select-country, #select-region {
            box-shadow: $shadow;
        }
    }
    h1 {
        font-size: 1.375em;
        font-weight: $bold;
        margin: 0;
        color: $main-text-color;
        span {
            font-weight: $regular;
        }
    }
    p {
        font-size: .875em;
        font-weight: $bold;
        color: $text-undertitle-color;
        margin: 5px 0;
        span {
            font-weight: $regular;
        }
    }
}
}
@media only screen and (min-width: 495px) {
header {
        .header-inputs {
            display: flex;
            margin-top: 20px;
            .v-input {
                margin: 0 !important;
            }
            #select-country {
                max-width: 340px;
                margin-right: 20px !important;
            }
            #select-region {
                margin-left: auto;
                max-width: 340px;
            }
        }
    }
}

</style>