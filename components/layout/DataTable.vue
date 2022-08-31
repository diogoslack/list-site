<template>
  <div>      
      <div class="table-responsive">        
        <table class="table table-striped table-hover table-sm align-middle table-responsive">
          <thead>
            <tr>
              <th v-for="(title, id) in titles" :key="id" scope="col">
                {{ title }}
              </th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(row, rid) in rows" :key="rid">
              <td v-for="(value, vid) in row" :key="vid" :class="{ inactive: ignoreFields.includes(vid) }">{{ value }}</td>
            </tr>
          </tbody>
        </table>
        <div v-if="pagination" class="text-end">
          <ul class="pagination overflow-auto">
            <li v-for="(val, id) in getPagination" :key="id" :class="{ 'current-page': val === pagination.currentPage }">
              <a v-if="val !== pagination.currentPage" class="link" @click="openPage(val)">{{ val }}</a>
              <span v-else>{{ val }}</span>
            </li>
          </ul>
        </div>
      </div>      
    </div>
</template>

<script>
export default {
  name: 'DataTable',
  props: {
    titles: {
      type: Array,
      required: true
    },
    rows: {
      type: Array,
      required: true
    },
    ignoreFields: {
      type: Array,
      required: false,
      default: () => []
    },
    pagination: {
      type: Object,
      required: false,
      default: () => {}
    },
  },
  computed: {    
    getRows () {
      return this.filter ? this.rows.filter(value => value.name.toLowerCase().includes(this.filter.toLowerCase())) : this.rows;
    },
    getPagination () {
      const pages = [];
      const totalPages = parseInt(this.pagination.totalPages);     
      for(let x = 1; x <= totalPages; x++) {
        pages.push(x);
      }
      return pages;      
    }
  },
  methods: {
    openPage(val) {
      this.$emit('change-page', val);
    },
  },
}
</script>

<style scoped>
  td > span > a {
    text-transform: uppercase;
    text-decoration: none;
  }
  .inactive {
    display: none;
  }  
  
  .pagination {
    width: 100%;
    display: inline-flex;
  }
  .pagination li {
    list-style: none;
    margin-right: 5px;
    color: black;
    padding: 8px 16px;
    text-decoration: none;
  }
  .pagination > .current-page {
    background-color: #4CAF50;
    color: white;
  }
  .link {
    cursor: pointer;
    text-decoration: underline;
    color: rgb(14, 62, 133);
  }
</style>
