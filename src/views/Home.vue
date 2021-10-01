<template>
  <main v-if="!loading">
    <DataTitle :text="title" :dataDate="dataDate" />
    <DataBoxes :stats="stats" />
    <CountrySelect @get-country="getCountryData" :countries="countries"/>
    <div class="text-center">
    <button @click="clearCountryData()" v-if="stats.Country" class="rounded mt-10 bg-green-600 text-white p-3 hover:bg-green-500">Clear Country</button>
    </div>
  </main>
  <main class="flex flex-col align-center justify-center text-center" v-else>
    <div class="text-grey-300 text-3xl mt-10 mb-6">Loading...</div>
    <img :src="loadingImage" class="w-24 m-auto" alt="Loading" />
  </main>
</template>

<script>
// @ is an alias to /src
import DataTitle from '@/components/DataTitle'
import DataBoxes from "@/components/DataBoxes";
import CountrySelect from "@/components/CountrySelect";
export default {
  name: "Home",
  components: {
    DataTitle,
    DataBoxes,
    CountrySelect
  },
  data() {
    return {
      loading: true,
      title: "Global",
      dataDate: "",
      stats: {},
      countries: [],
      loadingImage: require("../assets/hourglass.gif"),
    };
  },
  methods: {
    async fetchData() {
      const response = await fetch("https://api.covid19api.com/summary");
      const data = await response.json();
      return data;
    },
    getCountryData(country) {
      this.stats = country
      this.title = country.Country
    },
    async clearCountryData() {
      this.loading = true;
        const data = await this.fetchData();
        this.title = "Global";
        this.dataDate = data.Date
        this.stats = data.Global
        this.countries = data.Countries
        this.loading = false
    },    
  },
  async created() {
    const data = await this.fetchData();
    this.dataDate = data.Date
    this.stats = data.Global
    this.countries = data.Countries
    this.loading = false
  },
};
</script>