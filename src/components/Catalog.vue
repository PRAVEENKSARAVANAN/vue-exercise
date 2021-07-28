<template>
  <div>
    <input
      v-model="searchTerm"
      placeholder="search term"
    >
    <div class="catalog">
      <KCard
        v-for="service in filteredServices"
        :key="service.id"
        class="service"
      >
        <template slot="title">
          {{ service.name }}
        </template>
        <template slot="body">
          {{ service.description }}
        </template>
      </KCard>
    </div>
  </div>
</template>

<script>
import Vue from 'vue'
import KCard from '@kongponents/kcard'
import axios from 'axios'

export default Vue.extend({
  name: 'Catalog',
  components: {
    KCard
  },
  data () {
    return {
      services: [],
      filteredServices: [],
      searchTerm: ''
    }
  },
  watch: {
     searchTerm (val) {
      
      val = new RegExp(val, "gi");
      this.filteredServices = this.services.filter((item) => {
        if(item.name.search(val) > -1  || item.description.search(val) > -1){
          return item;
        }
        return;
      }
      )
  }},
  mounted () {
    this.fetchServices()
  },
  methods: {
    fetchServices () {
      axios.get('/api/service_packages').then((res) => {
        this.services = res.data
        this.filteredServices = res.data
      })
    }
  }
  
  })
</script>

<style lang="scss">
.catalog {
  display: flex;
  flex-wrap: wrap;
}
.service {
  width: 200px;
  margin: 10px;
}
</style>
