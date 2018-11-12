<template>
  <div id="graph">
    <div id="container">
      <div id="dayColumn">
        <div v-for="day in days" :key="day">{{ day }}</div>
      </div>
      <div id="canvas" >
        <div v-for="i in numCol" :key="i"> <!--starts from 1 ! -->
          <div v-if="i == 1  && index2month(i) == index2month(i+1) || index2month(i) != index2month(i-1) ">
            <div class="month">{{index2month(i)}}</div>
          </div>
          <div v-else><div class="empty"></div></div>
          <div class="graph-row" v-for="j in numRow(i)" :key="j"> 
            <div class="circle" v-bind:id="idCalc(i,j)" v-bind:style="circleColor(i,j)"></div>
            <b-tooltip v-bind:target="idCalc(i,j).toString()">
              <div v-if="activityDict[id2date(idCalc(i,j))]"> 
                {{activityDict[id2date(idCalc(i,j))]}} contributions on {{id2date(idCalc(i,j))}}
              </div>
              <div v-else>no contributions on {{id2date(idCalc(i,j))}}</div>     
            </b-tooltip>  
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "contribution-graph",
  props: {
    startDate: String,
    activity: Array,
    colors: Array
  },
  data: function() {
    return {
      numCol: 53
    };
  },
  methods: {
    showCont() {
      this.contNumber = "5";
    },
    idCalc: function(i, j) {
      return (i - 1) * 7 + j - 1;
    },
    calcYearDays: function() {
      var startDate = new Date(this.startDate);
      var day = startDate.getDate();
      var month = startDate.getMonth();
      var year = startDate.getFullYear() + 1;

      var endDate = new Date(year, month, day);
      var oneDay = 24 * 60 * 60 * 1000; // hours*minutes*seconds*milliseconds
      var days = Math.round(
        Math.abs((endDate.getTime() - startDate.getTime()) / oneDay)
      );
      return days;
    },
    id2date: function(id) {
      var result = new Date(this.startDate);
      result.setDate(result.getDate() + id);
      return result.toDateString();
    },
    index2month: function(i) {
      var monthTable = [
        "Jan",
        "Feb",
        "Mar",
        "Apr",
        "May",
        "Jun",
        "Jul",
        "Aug",
        "Sep",
        "Oct",
        "Nov",
        "Dec"
      ];
      var id = this.idCalc(i,1); //always first row
      var date = new Date(this.startDate);
      date.setDate(date.getDate() + id);
      var i = date.getMonth(); //0-11
      return monthTable[i];
    },

    numRow: function(i) {
      var row = 7;
      if (i == 53) {
        row = this.lastRow;
      }
      return row;
    },
    toValue: function(contNumber) {
      if (contNumber == 0 || this.contMax == 0) {
        return null;
      }
      var ratio = (contNumber / this.contMax) * 100;
      if (ratio <= 25) {
        return 0;
      } else if (ratio <= 50) {
        return 1;
      } else if (ratio <= 75) {
        return 2;
      } else {
        return 3;
      }
    },
    circleColor: function(i, j) {
      if (this.activityDict[this.id2date(this.idCalc(i, j))]) {
        return {
          "background-color": this.colors[
            this.toValue(this.activityDict[this.id2date(this.idCalc(i, j))])
          ]
        };
      } else {
        return {
          "background-color": "#ebedf0"
        };
      }
    }
  },
  computed: {
    // the last row has either 1 or 2 days, depending on leap years.
    lastRow: function() {
      var daysInYear = this.calcYearDays(this.startDate);
      var lastRow = daysInYear - 7 * 52;
      return lastRow;
    },
    activityDict: function() {
      var ad = {};
      for (let elem of this.contTable) {
        var date = new Date(elem[0]);
        var temp = date.toDateString();
        ad[temp] = elem[1];
      }
      return ad;
    },
    contTable: function() {
      var contTable = this.activity.slice(0); //copy of input
      var length = this.activity.length;
      var endDate = new Date(this.startDate);
      var daysInYear = this.calcYearDays();
      endDate.setDate(endDate.getDate() + daysInYear); 
      var startDate = new Date(this.startDate);
      //reverse loop to not mess up the index after splice
      for (let i = length - 1; i >= 0; i--) {
        var tempDate = new Date(this.activity[i][0]);
        if (tempDate < startDate || tempDate > endDate) {
          contTable.splice(i, 1);
        } //else
      }
      return contTable;
    },
    days: function() {
      var dayTable = [
        "Sun",
        "Mon",
        "Tue",
        "Wed",
        "Thu",
        "Fri",
        "Sat"
      ];
      var days = [];
      var startDate = new Date(this.startDate);
      var startDay = startDate.getDay();// Sunday - Saturday : 0 - 6
      for (let i = 0; i < 4; i++) {
        var mIndex = (startDay + i*2) % 7;
        days[i] = dayTable[mIndex];
      }
      return days;
    },
    contMax: function() {
      var length = this.contTable.length;
      var max = 0;
      for (let i = 0; i < length; i++) {
        var temp = this.contTable[i][1];
        max = Math.max(temp, max);
      }
      return max;
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
#graph {
  margin: auto;
  width: 50%;
}

#container {
  display: flex;
  flex-direction: row;
  #dayColumn {
    display: flex;
    flex-direction: column;
    width: 20px;
    height: 87px;
    justify-content: space-between;
    font-size: 11px;
    color: #414660;
    margin-right: 10px;
    margin-top: 20px;
  }
  #canvas {
    display: flex;
    flex-direction: row;
    .month {
      font-size: 11px;
      color: #414660;
      width: 10px;
      margin-bottom: 5px;
    }
    .empty {
      margin-bottom: 11px;
      color:transparent;
      border: 5px solid transparent;
    }
    .circle {
      width: 11px;
      height: 11px;
      border-radius: 50%;
      background-color: #ebedf0;
      margin: 1px;
    }
  }
} //container

</style>
