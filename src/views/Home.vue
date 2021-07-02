<template>
  <main v-if="!loading">
     <DataTitle :text="title" :dataDate="dataDate" :countryCode="countryCode"/>
     <DataBoxes :stats="stats" />
      
     <CountrySelect @get-country="getCountryData" :countries="countries" />

      <div v-if="selected">
        <DataGraph :country="title"/>
      </div>
     <div class="grid">
        <button @click="clearCountryData"
          v-if="stats.Country"
          class="bg-green-700 text-white justify-self-strech rounded p-3 mt-10 focus:outline-none hover:bg-green-600">
          Clear Country
        </button>
     </div>
     
  </main>


  <main class="flex flex-col align-center justify-center text-center" v-else>
    <div class="text-gray-500 text-3xl mt-10 mb-6">
      Fetching Data
    </div>
    <img :src="loadingImage" class="w-24 m-auto" alt="loading">
  </main>
</template>


<script>
import DataBoxes from '@/components/DataBoxes'
import DataTitle from '@/components/DataTitle'
import CountrySelect from '@/components/CountrySelect'
import DataGraph from '@/components/DataGraph'

export default {
  name: 'Home',
  components: {
    DataTitle,
    DataBoxes,
    CountrySelect,
    DataGraph,
  },
  data() {
    return {
      loading: true,
      selected: false,
      title: 'Global',
      dataDate: '',
      stats: {},
      countries: [],
      countryCode: '',
      loadingImage: require('../assets/search.gif')
    }
  },
  methods: {
    async fetchCovidData() {
      const res = await fetch('https://api.covid19api.com/summary');
      const data =  await res.json();

      return data;
    },
    getCountryData(country) {
      this.selected = true;
      this.stats = country;
      this.title = country.Country;
      this.countryCode = country.CountryCode.toLowerCase();
    },
    async clearCountryData() {
      this.loading = true;
      const data = await this.fetchCovidData();
      this.title = 'Global';
      this.stats = data.Global;
      this.countryCode = '';
      this.loading = false;
      this.selected = false;
    }
  },
  async created() {
    const data = await this.fetchCovidData();
    this.dataDate = data.Date;
    this.stats = data.Global;
    this.countries = data.Countries;
    this.loading = false;
  },
}
</script>
