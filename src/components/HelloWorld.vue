<template>
  <div>
    <h1>Listado de Países</h1>
    <a-table :columns="columns" :data-source="countries" class="country-table" :rowKey="record => record.key">

      <template #flag="{ text }">
        <img :src="text" alt="flag" class="flag-image">
      </template>
      
      <template #name="{ text }">
        <div>{{ text }}, {{ countries.find(c => c.name.common === text)?.capital[0] }}</div>
      </template>

      <template #action="{ record }">
        <a-button type="primary" size="small" @click="showDetails(record)">Ver detalles</a-button>
      </template>

    </a-table>

    <a-modal title="Detalles del país" :visible="modalVisible" @cancel="closeModal">
<img v-if="selectedCountry" :src="selectedCountry.flags.png" alt="Bandera" class="flag-image" style="max-width: 100%; height: auto;">
<p v-if="selectedCountry" style="margin-top: 10px; font-size: 18px;"><strong>Región: </strong>{{ selectedCountry.region }}</p>
<p v-if="selectedCountry" style="margin-top: 10px; font-size: 18px;"><strong>Nombre: </strong>{{ selectedCountry.name.common }}</p>
<p v-if="selectedCountry" style="margin-top: 10px; font-size: 18px;"><strong>Capital: </strong>{{ selectedCountry.capital[0] }}</p>
<p v-if="selectedCountry" style="margin-top: 10px; font-size: 18px;"><strong>Lenguajes: </strong>{{ selectedCountry.languages }}</p>
<p v-if="selectedCountry" style="margin-top: 10px; font-size: 18px;"><strong>Código Numérico: </strong>{{ selectedCountry.numericCode }}</p>
<p v-if="selectedCountry" style="margin-top: 10px; font-size: 18px;"><strong>Traducciones: </strong>{{ selectedCountry.translations }}</p>
</a-modal>








  </div>
</template>

<script>
import axios from 'axios';
export default {
  data() {
    return {
      countries: [],
      modalVisible: false,
      selectedCountry: null
    }
  },
  mounted() {
    axios.get('https://restcountries.com/v3.1/all')
      .then(response => {
        this.countries = response.data.map(country => {
          return {
            ...country,
            flagUrl: country.flags.svg,
            key: country.alpha3Code
            
          }
        });
      })
      .catch(error => {
        console.log(error);
      });
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
          dataIndex: 'name.common',
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
  methods: {
    showDetails(record) {
      this.selectedCountry = record;
      this.modalVisible = true;
    },
    closeModal() {
      this.modalVisible = false;
    }
  }
}
</script>

<style scoped>
.flag-image {
  width: 30px;
  height: auto;
}
.country-table {
  margin-top: 20px;
}
</style>
