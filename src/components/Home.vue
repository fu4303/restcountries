<script>
import axios from 'axios';

export default {
  name: 'home',
  props: [ 'isDarkTheme' ],
  data () {
    return {
      pending: false,
      error: null,
      countryInfo: null,
      search: '',   
      darkMode: false,
    }
  },
  mounted () {
    this.pending = true;
    axios
      .get('https://restcountries.eu/rest/v2/all')
      .then(response => (this.countryInfo = response.data))
      .catch(error => (this.error = error ))
      .finally( () => { this.pending = false });
  },
 
  filters: {
    formatNumbers (value) {
      return `${value.toLocaleString()}`
    }
  },
  computed: {
    filteredCountries: function () {
      return this.countryInfo.filter((country) => {
        if (this.region === '' ) {
          return country.name.toLowerCase().match(this.search.toLowerCase());
        } else if (this.search !== '') {
          return country.name.toLowerCase().match(this.search.toLowerCase());
        } else {
          return ('blbla');
        }
      })
    }
  }, 
 
}
</script>
<template>
  <div class="home" :class="{ darkTheme : isDarkTheme }">
    <div class="searchBar">
      <div class="searchContainer">
        <i class="fas fa-search searchIcon"></i>
        <input 
          class="searchInput" 
          type="text" 
          v-model="search"
          aria-label="Search for a country..."
          placeholder="Search for a country..."
        />
        <ul class="searchResults"></ul>
      </div>

    </div>
    <h1 v-if="error !== null">Sorry, an error has occurred {{error}}</h1> 
    <div class="loaderFlex"><div v-if="pending" class="loader"></div></div>

    <div v-if="countryInfo" class="tileGrid" >
      <div v-for="country in filteredCountries" class="countryTile" v-bind:key="country.id">
        <router-link 
          :to="{ name: 'country-detail', params: {country: country.name }}" 
          class="linkTile"
        >
          <img v-bind:src="country.flag" alt="Country Flag" class="flag">
          <div class="text">
            <h1>{{ country.name }}</h1>
          </div>
       </router-link>
      </div>
    </div>
  </div>
</template>

<style scoped>

.home {
  background-color: #f9f4ef
}

.searchBar {
  padding: 35px 75px;
  display: flex;
  justify-content: center;
  max-width: 1400px;
  margin: 0 auto;
}

.searchContainer {
  border: none;
  border-radius: 5px;
  box-shadow: 1px 1px 7px 0px rgb(140 119 81);
  width: 500px;
  height: 50px;
  background-color: #fff;
  display: flex;
  align-items: center;
  padding-left: 30px;

}

.searchIcon {
  font-size: 16px;
  color: #848484;
  padding-right: 30px;
}

.searchInput {
  border: none;
  font-size: 16px;
  font-family: 'Nunito Sans', sans-serif;
  width: 420px;
  
}


input[type="radio"] {
  -webkit-appearance: radio;
}

.loader {
  border: 16px solid #f3f3f3;
  border-top: 16px solid #2b3845; 
  border-radius: 50%;
  width: 120px;
  height: 120px;
  animation: spin 2s linear infinite;
  margin-bottom: 100px;
}
@keyframes spin {
  0% { transform: rotate(0deg); }
  100% { transform: rotate(360deg); }
}
.loaderFlex {
  display: flex;
  justify-content: center;
}

.tileGrid {
  display: flex;
  flex-wrap: wrap;
  padding: 25px 50px 0;
  margin: 0 auto;
  max-width: 1450px;
}

.countryTile {
  display: inline-block;
  background-color: #eaddcf;
  width: 300px;
  box-shadow: 1px 1px 7px 0px rgb(0, 0, 0, 0.1);
  border-radius: 13px;
  cursor: pointer;
  text-align: left;
  margin: 0 15px 40px;
  text-decoration: none;
  color: inherit;
  -webkit-animation: fadein 1s; /* Safari, Chrome and Opera > 12.1 */
       -moz-animation: fadein 1s; /* Firefox < 16 */
        -ms-animation: fadein 1s; /* Internet Explorer */
         -o-animation: fadein 1s; /* Opera < 12.1 */
            animation: fadein 1s;
}

.countryTile:hover {
transform: scale(1.1); 
}

@keyframes fadein {
    from { opacity: 0; }
    to   { opacity: 1; }
}


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

.linkTile {
  display: inline-block;
  width: 300px;
  text-decoration: none;
  color: inherit;
}

.flag {
  height: 180px;
  width: 300px;
  border-radius: 13px 13px 0 0;
}

.text {
  padding-left: 30px;
  padding-right: 20px;
}

.text h1 {
  font-size: 20px;
}

.text p span {
  font-weight: 600;
}

.text p {
  line-height: 1;
}

::placeholder {
  font-weight: 600;
}

.darkTheme,
.darkTheme .dropdownUL li:hover  {
  background-color: #202c36;
}

.darkTheme .searchContainer, 
.darkTheme .searchInput,
.darkTheme .dropdownBtn,
.darkTheme .dropdownUL,
.darkTheme .countryTile {
  background-color: #2b3845;
}

.darkTheme h1,
.darkTheme p,
.darkTheme .searchIcon, 
.darkTheme .searchInput, 
.darkTheme ::placeholder,
.darkTheme .dropdownBtn,
.darkTheme .dropdownUL {
  color: #fff;
}

.darkTheme .loader {
  border: 16px solid #2b3845;
  border-top: 16px solid #f3f3f3; 
}


@media (max-width: 875px) {
  .tileGrid {
    justify-content: center;
    padding-left: 50px;
  }

  .searchBar {
    flex-direction: column;
    padding: 25px 15px;
  }

  .searchContainer {
    width: inherit;
  }

  .dropdownDiv {
    margin-top: 40px;
  }
}

</style>
