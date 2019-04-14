<template> 
  <div id="app">
    <h1>Countries of the World</h1>
    <table>
      <thead>
        <tr>
          <th @click="sort('name')">Country</th>
          <th class="th">Flag</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="country in sortedCountries" :key="country.name">
          <td>{{country.name}}</td>
          <td class="flag"><img :src="country.flag"/></td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  name: 'app',
  data () {
    return {
      countries: [],
      currentSort: 'name',
      currentSortDir: 'asc'
    }
  },
  methods: {
    sort: function (s) {
      if (s === this.currentSort) {
        this.currentSortDir = this.currentSortDir === 'asc' ? 'desc' : 'asc'
      }
      this.currentSort = s
    }
  },
  computed: {
    sortedCountries () {
      return this.countries.sort((a, b) => {
        let modifier = 1
        if (this.currentSortDir === 'desc') modifier = -1
        if (a[this.currentSort] < b[this.currentSort]) return -1 * modifier
        if (a[this.currentSort] > b[this.currentSort]) return 1 * modifier
        return 0
      })
    }
  },
  async created () {
    try {
      const result = await axios.get(`http://localhost:3000/countries`)
      this.countries = result.data
    } catch (e) {
      console.error(e)
    }
  }
}
</script>

<style>
#app {
  display:grid;
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  justify-content: center;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
table, td {
  margin-left: 50px;
  text-align: center;
  width: 200px;
}
th {
  cursor: pointer;
  margin-top: 10px;
  width: 200px;
}
h1 {
  text-align: center;
}
.flag {
  display: grid;
  margin: 10px;
  max-height: 10px;
  width: 100%;
}
</style>