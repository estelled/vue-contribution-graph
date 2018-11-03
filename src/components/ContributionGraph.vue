<template>
  <div id="graph">
    <button v-on:click="showCont()">show contributions</button>
    <p>{{ contNumber}}<p>
    <p>num colonne : {{numCol}}</p>
    <div id="monthRow">
      <div v-for="month in months" :key="month">{{ month }}</div>
    </div>
    <div id="container">
      <div id="dayColumn">
        <span>Mon</span>
        <span>Wed</span>
        <span>Fri</span>
      </div>
      <!-- drawGraph -->
      <div id="canvas">
        <div class="graph-col" v-for="i in numCol" :key="i"> <!--starts from 1 ! -->
          <div class="graph-row" v-for="j in numRow" :key="j"> 
            <div class= "circle" v-bind:id="idCalc(i,j)"></div>
            <b-tooltip v-bind:target="idCalc(i,j).toString()" >{{activity[0][1].toString()}} contributions on {{activity[0][0].toString()}}</b-tooltip>  
          </div>
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
      months: ["Jan","Feb","Mar","Apr","May","Jun","Jul","Aug","Sep","Oct","Nov","Dec"],
    };
  },
  methods: {
    showCont() {
      this.contNumber = '5'
    },
    idCalc: function(i,j) {
      return (i-1)*7+j
    },
  },
  computed: {
    numCol: function() {
      return Math.floor(365 / this.numRow)
    } 
  }
}

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
#graph {
  margin: auto;
  width: 50%;
}
#monthRow {
  display :flex;
  flex-direction: row;
  width: 650px;
  justify-content: space-between;
  font-size: 11px;
  color : #414660; 
  margin-left: 40px;  
  margin-bottom: 10px;
}
#container {
  display: flex;
  flex-direction: row;
  #dayColumn {
    display: flex;
    flex-direction: column;
    width: 20px;
    height: 85px;
    justify-content: space-between;
    font-size: 11px;
    color: #414660;
    margin-right: 10px;
  }
  #canvas {
    display: flex;
    flex-direction: row;
    .circle {
      width: 11px;
      height: 11px;
      border-radius: 50%;
      background-color: #EBEDF0;
      margin : 1px;
    }
  }
}//container



</style>
