<template>
  <div id="canvas">
    <button v-on:click="showCont()">show contributions</button>
    <p>{{ contNumber}}<p>
    <p>num colonne : {{numCol}}</p>
    <!-- drawGraph -->
    <div class="graph-col" v-for="i in numCol" :key="i"> <!--starts from 1 ! -->
      <div class="graph-row" v-for="j in numRow" :key="j">
        <div class= "circle" 
          v-b-tooltip.hover
          title="I'm a tooltip!" 
          v-bind:id="(i-1)*7+j">
        </div>  
      </div>
    </div>
  </div>
</template>

<script>

export default {
  name: 'contribution-graph',
  props: {
    startDate: String,
    activity: Array,
    colors: Array,
  },
  data: function () {
    return {
      numRow: 7,
      contNumber: 'no contributions',
    };
  },
  methods: {
    showCont() {
      this.contNumber = '5'
    },
  },
  computed: {
    numCol: function() {
      return Math.floor(365 / this.numRow)
    } ,
  },
}

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
  #canvas {
    display: flex;
    flex-direction: row;
    .circle{
    width: 11px;
    height: 11px;
    border-radius: 50%;
    background-color: #EBEDF0;
    margin : 1px;
    }
  } 



</style>
