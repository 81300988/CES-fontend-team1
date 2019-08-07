
<template>
  <div>
    <div class="row" style="background-color:lavender;">
      <div class="col-sm-4 margin">
        <label for="movie">Depature</label>
        <v-select placeholder="Depature" v-model="Depature" label="name" :filterable="false" :options="options" @search="onSearch">
        </v-select>
      </div>
      <div class="col-sm-4 margin">
        <label for="movie">Depature</label>
        <v-select placeholder="Depature" v-model="Depature" label="name" :filterable="false" :options="options" @search="onSearch">
        </v-select>
      </div>
      <div class="col-sm-4 margin">
        <label for="movie">Depature</label>
        <v-select placeholder="Depature" v-model="Depature" label="name" :filterable="false" :options="options" @search="onSearch">
        </v-select>
      </div>
    </div>
    

  </div>
</template>

<script>
import axios from "axios";
import _ from 'lodash'
import vSelect from "vue-select"

export default {
  data() {
    return {
        options: [],
        results: [],
        Depature: null
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
        res.json().then(json => (vm.results = json.cities)); 
        console.log(vm.results);
        vm.options = vm.results.filter(function(item){
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

.margin {
  margin: 0px;
}
.row1 {
  width: 500px;
}
</style>