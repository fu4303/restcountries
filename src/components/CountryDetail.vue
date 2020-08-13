<script>
import axios from 'axios';
export default {
  name: 'country-detail',
  props: [ 'isDarkTheme' ],
  data () {
    return {
      pending: false,
      error: null,
      countryInfo: null,
      alpha3Code: [],
      alpha3CodetoString: [],
    }
  },
  mounted () {
    this.pending = true;
    axios
      .get(`https://restcountries.eu/rest/v2/name/${this.$route.params.country}?fullText=true`)
      .then((response) => {
        (this.countryInfo = response.data)
        this.alpha3CodetoString = this.alpha3Code.join(';');
      })
      .catch(error => (this.error = error ))
      .finally( () => { this.pending = false });
  },
  filters: {
    formatNumbers (value) {
      return `${value.toLocaleString()}`
    }
  },
 
}
</script>

<template>
  <div class="country-detail" :class="{ darkTheme : isDarkTheme }">
    <a @click="$router.go(-1)" class="backBtn"><i class="fas fa-arrow-left" /> Go back</a>
    <h1 v-if="error !== null">Sorry, an error has occurred {{error}}</h1>
    <div class="loaderFlex"><div v-if="pending" class="loader"></div></div>
    <div v-for="country in countryInfo" class="countryTile" v-bind:key="country.id">
      <img v-bind:src="country.flag" alt="Country Flag" class="flag">
      <div class="country-details">
        <h1>{{country.name}}</h1>
        <div class="listDiv">
          <ul>
            <li><span>Population:</span> {{country.population | formatNumbers }}</li>
            <li><span>Capital:</span> {{country.capital}}</li>
             <li><span>Iso:</span> {{country.alpha3Code}}</li>
          </ul>
          <ul>
           
            <li><span>Currencies:</span> {{country.currencies['0'].name}}</li>
            <li><span>Languages:</span> 
              <span 
                v-for="(language, index) in country.languages" 
                v-bind:key="index" 
                class="languages">
                {{language.name}}<span v-if="index + 1 < country.languages.length">, </span>
              </span>
            </li>
          </ul>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>

.country-detail {
  background: #f9f4ef;
  max-width: 1400px;
  margin: 0 auto;
  padding: 75px;
  font-size: 16px;
}

.loader {
  border: 16px solid #f3f3f3;
  border-top: 16px solid #2b3845; 
  border-radius: 50%;
  width: 120px;
  height: 120px;
  animation: spin 2s linear infinite;
  transition: 1s ease;
}
.borders .loader {
  width: 60px;
  height: 60px;
}
@keyframes spin {
  0% { transform: rotate(0deg); }
  100% { transform: rotate(360deg); }
}
.loaderFlex {
  display: flex;
  justify-content: center;
}

.backBtn, .borderLinks {
  display: block;
  background: #8c7851;
  width: 130px;
  box-shadow: 1px 1px 7px 0px rgb(0, 0, 0, 0.20);
  cursor: pointer;
  border-radius: 5px;
  text-decoration: none;
  color: #fff;
  padding: 10px 0;
  text-align: center;
  font-weight: 700;
}

.fa-arrow-left {
  padding-right: 7px;
}

.countryTile {
  display: flex;
  padding-top: 10px;
}

.countryTile {
  -webkit-animation: fadein 1s; /* Safari, Chrome and Opera > 12.1 */
       -moz-animation: fadein 1s; /* Firefox < 16 */
        -ms-animation: fadein 1s; /* Internet Explorer */
         -o-animation: fadein 1s; /* Opera < 12.1 */
            animation: fadein 1s;
}
@keyframes fadein {
    from { opacity: 0; }
    to   { opacity: 1; }
}

/* Firefox < 16 */
@-moz-keyframes fadein {
    from { opacity: 0; }
    to   { opacity: 1; }
}

@-webkit-keyframes fadein {
    from { opacity: 0; }
    to   { opacity: 1; }
}

@-ms-keyframes fadein {
    from { opacity: 0; }
    to   { opacity: 1; }
}

.flag {
  max-width: 570px;
  min-width: 350px;
  padding-bottom: 10px;
  width: 100%;
  height: 100%;
}

.country-details {
  text-align: left;
  min-width: 400px;
  max-width: 650px;
  margin: 0 0 0 auto;
  width: 100%;
  padding-left: 40px;
}

.listDiv {
  display: flex;
}

.country-details ul {
  list-style: none;
  text-align: left;
  line-height: 32px;
  padding-left: 0;
  margin: 0 auto 0 0;
}

.country-details ul:last-child {
  margin: 0 0 0 auto;
}

li span {
  font-weight: 600;
}

.languages, .languages span {
  font-weight: 400;
}

.darkTheme  {
  background-color: #202c36;
}

.darkTheme .borderLinks,
.darkTheme .backBtn {
  background-color: #2b3845;
}

.darkTheme h1,
.darkTheme p,
.darkTheme span,
.darkTheme li,
.darkTheme .borderLinks,
.darkTheme .backBtn {
  color: #fff;
}

.darkTheme .loader {
  border: 16px solid #2b3845;
  border-top: 16px solid #f3f3f3; 
}


@media (max-width: 875px) {
  .country-detail {
    padding: 25px;
  }

  .backBtn {
    margin-top: 22px;
  }

  .countryTile {
    flex-direction: column;
  }

  .flag {
    min-width: 325px;
    height: 100%;
    padding-bottom: 20px;
  }

  .country-details {
    padding-left: 0;
    min-width: 325px;
  }

  .listDiv {
    flex-direction: column;
  }

  .country-details ul:last-child {
    margin: inherit;
    padding-top: 40px;
  }
}

</style>
