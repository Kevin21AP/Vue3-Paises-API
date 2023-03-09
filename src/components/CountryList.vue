<template>
  <div>
    <input type="text" v-model="searchText" @input="searchCountries" class="search-country">
    <a-table :columns="columns" :data-source="countries" class="country-table" :rowKey="record => record.key">

      <template #flag="{ text }">
        <img :src="text" alt="flag" class="flag-image">
      </template>

      <template #action="{ record }">
        <a-button type="primary" size="small" @click="showDetails(record)">Ver Pais</a-button>
      </template>

    </a-table>
    
    <a-modal v-if="modalVisible" v-model:visible="modalVisible">
      <h2>{{ selectedCountry.name }}</h2>
      <img :src="selectedCountry.flagUrl" alt="Bandera" class="flag-image-modal">
      <p><strong>Capital:</strong> {{ selectedCountry.capital }}</p>
      <p><strong>Población:</strong> {{ selectedCountry.population }}</p>
      <p><strong>Lenguaje:</strong> {{ selectedCountry.languages }}</p>
    </a-modal>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      searchText: '',
      countries: [],
      modalVisible: false,
      selectedCountry: {}
    }
  },
  methods: {
    
    searchCountries() {
      axios.get(`https://restcountries.com/v3.1/name/${this.searchText}`)
        .then(response => {
          this.countries = response.data.map(country => {
            return {
              key: country.cca3,
              name:  country.name.common,
              population: country.population,
              languages:country.languages,
              flagUrl: country.flags.png,
              capital: country.capital?.[0]
            }
          });
        })
        .catch(error => console.error(error));
    },
    
    showDetails(record) {
      this.selectedCountry = record;
      this.modalVisible = true;
    }
    
  },
  computed: {
    columns() {
      return [
        {
          title: 'Bandera',
          dataIndex: 'flagUrl',
          slots: { customRender: 'flag' },
          width: 100,
          align: 'center'
        },
        {
          title: 'Nombre',
          dataIndex: 'name',
          key: 'name',
          slots: { customRender: 'name' },
          align: 'center'
        },
        {
          title: 'Población',
          dataIndex: 'population',
          key: 'population',
          align: 'center'
        },
        {
          title: 'Acción',
          dataIndex: '',
          key: 'action',
          slots: { customRender: 'action' },
          width: 100,
          align: 'center'
        },
      ];
    }
  },
}
</script>

<style scoped>
.flag-image {
  width: 30px;
  height: auto;
}
.flag-image-modal {
  width: 80px;
  height: auto;
}
.country-table {
  margin-top: 20px;
}
.search-country
{ 
border: 1px solid #ccc;
padding: 5px;
background: #fff;}
</style>
