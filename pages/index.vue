<template>
  <main class="bg-light col-md-9 ms-sm-auto col-lg-10 px-md-4">
     <div class="d-flex justify-content-between flex-wrap flex-md-nowrap align-items-center pt-3 pb-2 mb-3 border-bottom">
      <h1 class="h2">Products</h1>      
    </div>

    <search-form :locations="locations" @do-search="doSearch($event)" />
    <hr />
    <div v-show="alertMessage" class="alert alert-danger" role="alert">{{ alertMessage }}</div>
    <data-table 
      v-show="contentData"
      :titles="titles"
      :rows="getRows"
      :ignore-fields="ignoreFields"
      :pagination="pagination"
      @change-page="changePage($event)"
    />    
  </main>
</template>

<script>
import DataTable from '~/components/layout/DataTable.vue';
import SearchForm from '~/components/layout/SearchForm.vue';

export default {
  name: 'ProductPage',
  components: {
    DataTable,
    SearchForm
  },
  data() {
    return {
      titles: ['Model','RAM','HDD','Location','Price'],
      ignoreFields: ['id'],
      contentData: [],
      locations: [],
      pagination: {},
      page: 1,
      selectedLocation: '',
      selectedRam: [],
      selectedHddType: '',
      alertMessage: '',
      selectedStorageFrom: '',
      selectedStorageTo: '',
    }
  },
  computed: {
    getRows () {
      return this.contentData.map(val => { 
        return {
          id: val.id, model: val.model, ram: val.ram, hdd: val.storage, location: val.location, price: `${val.currency}${val.price}`,
        };
      })
    },
  },
  async mounted() {
    await this.getLocation();
  },
  methods: {
    async getLocation() {
      try {
        const { result } = await this.$axios.$get(`/product/location`);
        this.locations = result;
      } catch(error) {
        console.error(error);
      }
    },
    async doSearch (e = null) {
      try {
        if (e) {
          this.setResult(e);
        }        
        const querystr = this.getSearchQueryString();
        const { result, pagination } = await this.$axios.$get(`/product${querystr}`);
        this.contentData = result;
        this.alertMessage = !result || result.length < 1 ? 'No results found for your search criteria.' : '';
        this.pagination = pagination;
      } catch(error) {
        console.error(error);
        this.alertMessage = error;
      }      
    },
    async changePage(e) {
      this.page = parseInt(e) > 0 ? parseInt(e) : 1;
      await this.doSearch();
    },
    setResult(val) {
      this.page = 1;
      this.selectedLocation = val.location || '';
      this.selectedHddType = val.harddisktype || '';
      this.selectedRam = val.ram && val.ram.length > 0 ? val.ram : [];
      this.selectedStorageFrom = val.storageFrom || '';
      this.selectedStorageTo = val.storageTo || '';
    },
    getSearchQueryString() {
      const qs = [`?page=${this.page}`];
       if (this.selectedLocation) {
        qs.push(`location=${this.selectedLocation}`);
      }
      if (this.selectedHddType) {
        qs.push(`harddisktype=${this.selectedHddType}`);
      }
      if (this.selectedRam && this.selectedRam.length > 0) {
        qs.push(`ram=${this.selectedRam.join(',')}`);
      }
      if (this.selectedStorageFrom && this.selectedStorageTo) {
        qs.push(`storage=${this.selectedStorageFrom},${this.selectedStorageTo}`);
      }
      return qs.join('&');
    }
  },
}
</script>
