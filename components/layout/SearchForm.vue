<template>
    <div class="table-responsive">        
      <form class="overflow-hidden">
        <div class="row">
          <div class="col-sm-6 col-lg-6 text-start">
            <span>LOCATION:</span>
            <select v-model="selectedLocation" class="form-select" name="location" aria-label="Location selection">
              <option selected></option>
              <option v-for="(location, idx) in locations" :key="idx">{{ location.location }}</option>
            </select>
          </div>
          <div class="col-sm-6 col-lg-6 text-start">
            <span>RAM:</span><br />
            <div v-for="(ram, idx) in ramOptions" :key="idx" class="form-check form-check-inline">
              <input id="ram" v-model="selectedRam" class="form-check-input" type="checkbox" :name="ram" :value="ram">
              <label class="form-check-label" for="ram">
                {{ ram }}
              </label>
            </div>
          </div>
        </div>
        <div class="row">
          <div class="col-sm-6 col-lg-6 text-start">
            <label for="storagefrom" class="form-label">STORAGE FROM <b>{{ selectedStorageFrom }}</b></label>
            <input id="storagefrom" v-model="storageFrom" type="range" :min="storageFromMin" :max="storageFromMax" step="1" name="storagefrom" class="form-range" @change="setStorageRange">
            
            <label for="storageto" class="form-label">TO <b>{{ selectedStorageTo }}</b></label>
            <input id="storageto" v-model="storageTo" type="range" :min="storageToMin" :max="storageToMax" step="1" name="storageto" class="form-range" @change="setStorageRange">            
            
          </div>
          <div class="col-sm-6 col-lg-6 text-start">
            <span>HARDDISK TYPE:</span><br />
            <select v-model="selectedHddType" class="form-select" name="harddisktype" aria-label="Harddisk type selection">
              <option selected></option>
              <option v-for="(hdtype, idx) in hdTypeOptions" :key="idx">{{ hdtype }}</option>
            </select>
          </div>          
        </div>
        <div class="row mb-2">
          <div class="col-sm-12 col-lg-12 mt-4 text-center">
            <button type="button" class="btn btn-primary" @click="doSearch()">Search</button>
          </div>  
        </div>
      </form>
  </div>
</template>

<script>
import inputData from '~/components/layout/data/searchData.json';

export default {
  name: 'SearchForm',
  props: {
    locations: {
      type: Array,
      required: true
    }, 
  },
  data () {
    return {
      ramOptions: inputData.ram || [],
      hdTypeOptions: inputData.hardDiskType || [],
      storageOptions: inputData.storage || [],
      storageFrom: 0,
      storageFromMin: 0,
      storageFromMax: 11,
      storageTo: 0,
      storageToMin: 0,
      storageToMax: 11,
      selectedStorageFrom: '',
      selectedStorageTo: '',
      selectedLocation: '',
      selectedRam: [],
      selectedHddType: '',
    }
  },
  computed: {    
    getStorageFrom () {
      return this.storageFrom;
    },
    getStorageTo () {
      return this.storageTo;
    }
  },
  mounted() {
    this.storageFromMin = 0;
    this.storageFromMax = inputData.storage.length - 1;
    this.storageToMin = 0;
    this.storageToMax =  inputData.storage.length - 1;
  },
  methods: {
    doSearch() {
      const val = {};
      if (this.selectedLocation) {
        val.location = this.selectedLocation;
      }
      if (this.selectedHddType) {
        val.harddisktype = this.selectedHddType;
      }
      if (this.selectedRam && this.selectedRam.length > 0) {
        val.ram = this.selectedRam;
      }
      if (this.selectedStorageFrom && this.selectedStorageTo) {
        val.storageFrom = this.selectedStorageFrom;
        val.storageTo = this.selectedStorageTo;
      }
      this.$emit('do-search', val);
    },
    setStorageRange() {
      if (parseInt(this.storageTo) < parseInt(this.storageFrom)) {
        this.storageTo = this.storageFrom;
      }
      this.selectedStorageFrom = this.storageOptions[this.storageFrom];
      this.selectedStorageTo = this.storageOptions[this.storageTo];
    },
  },
}
</script>

<style scoped>
</style>
