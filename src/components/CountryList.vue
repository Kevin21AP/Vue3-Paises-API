<template>
  <div>
    <h1>Listado de Países</h1>
    <table class="country-table">
      <thead>
        <tr>
          <th>Nombre</th>
          <th>Código de país</th>
          <th>Capital</th>
          <th>Región</th>
          <th>Población</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="country in countries" :key="country.name.common">
          <td class="country-name">{{ country.name.common }}</td>
          <td class="country-code">{{ country.cca2 }}</td>
          <td class="country-capital">{{ country.capital }}</td>
          <td class="country-region">{{ country.region }}</td>
          <td class="country-population">{{ country.population }}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      countries: []
    }
  },
  mounted() {
    axios.get('https://restcountries.com/v3.1/all')
      .then(response => {
        this.countries = response.data;
      })
      .catch(error => {
        console.log(error);
      });
  }
}
</script>
