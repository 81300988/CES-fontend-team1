<template>
  <div>
    <h1>Vue Select - Ajax</h1>
    <v-select label="name" :filterable="false" :options="options" @search="onSearch">
      <template slot="no-options">
        type to search GitHub repositories..
      </template>
      <template slot="option" slot-scope="option">
        <div class="d-center">
          <img :src='option.owner.avatar_url'/> 
          {{ option.full_name }}
          </div>
      </template>
      <template slot="selected-option" slot-scope="option">
        <div class="selected d-center">
          <img :src='option.owner.avatar_url'/> 
          {{ option.full_name }}
        </div>
      </template>
    </v-select>
  </div>
  
</template>

<script>
import axios from "axios";
import _ from 'lodash'
import vSelect from "vue-select"

export default {
  data() {
    return {
        options: []
    }
    
  },
  methods: {
    onSearch(search, loading) {
      loading(true);
      this.search(loading, search, this);
    },
    search: _.debounce((loading, search, vm) => {
      fetch(
        `http://localhost:53842/Home/Details`
      ).then(res => {
        console.log(res);
        res.json().then(json => (vm.options = json.cities)); 
        console.log(vm.options);
        vm.options = vm.options.filter(function(item){
            return item.includes(search);
        });
        console.log(vm.options);
        loading(false);
      });
    }, 350)
  },
  components:{
    vSelect
  }
};
</script>

<style scoped>
img {
  height: auto;
  max-width: 2.5rem;
  margin-right: 1rem;
}

.d-center {
  display: flex;
  align-items: center;
}

.selected img {
  width: auto;
  max-height: 23px;
  margin-right: 0.5rem;
}

.v-select .dropdown li {
  border-bottom: 1px solid rgba(112, 128, 144, 0.1);
}

.v-select .dropdown li:last-child {
  border-bottom: none;
}

.v-select .dropdown li a {
  padding: 10px 20px;
  width: 100%;
  font-size: 1.25em;
  color: #3c3c3c;
}

.v-select .dropdown-menu .active > a {
  color: #fff;
}


</style>