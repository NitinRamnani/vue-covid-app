<template>
  <main v-if="!loading">
    <DataTitle :text="title" :dataDate="dataDate" />
  </main>
  <main class="flex flex-col align-center justify-center text-center" v-else>
    <div class="text-grey-300 text-3xl mt-10 mb-6">Loading...</div>
    <img :src="loadingImage" class="w-24 m-auto" alt="Loading" />
  </main>
</template>

<script>
// @ is an alias to /src
import DataTitle from '@/components/DataTitle'
export default {
  name: "Home",
  components: {
    DataTitle
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