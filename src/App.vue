<template>
  <div id="app">
    <contribution-graph 
      startDate="'2018-08-01'"
      :activity="[['2019-08-03', '86'], ['2015-06-04', '99'], ['2018-06-12', '24'], ['2018-08-01', '8'], ['2019-02-24', '35'], ['2019-07-24', '2'], ['2018-12-24', '34'], ['2019-07-31', '18']]"
      :colors="['#BBE3F4', '#82C9EB', '#039BE5', '#0A25B1']">
    </contribution-graph>
  </div>
</template>


<script>
import ContributionGraph from './components/ContributionGraph.vue'
import Vue from 'vue'
import BootstrapVue from 'bootstrap-vue'

Vue.use(BootstrapVue);

export default {
  name: 'app',
  components: {
    'contribution-graph': ContributionGraph
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
