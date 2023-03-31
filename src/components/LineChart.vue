<template>
  <div style="display: flex; flex-direction: row;">
  <div class="chart-container" style="width:80%; height:60vh; overflow:hidden">
    <Line
      id="my-chart-id"
      :options="chartOptions"
      :data="chartData"
      v-if="loaded"
      :style="{width: '100%', height: '100%'}"
    />
    <div v-if="!loaded">Loading ...</div>
  </div>
  <form @submit.prevent="updateData" style="display: flex;flex-direction: column;align-items: start; margin: 0 20px; row-gap: 10px;">
      <label>Start date:</label>
      <input type="date" v-model="startDate">
      <label>End date:</label>
      <input type="date" v-model="endDate">
      <label>Region:</label>
      <select v-model="region">
        <option value="US">United States</option>
        <option value="CA">Canada</option>
        <option value="GB">United Kingdom</option>
        <option value="DE">Germany</option>
        <!-- Add more regions as needed -->
      </select>
      <label>Product:</label>
      <select v-model="product">
        <option value="19">Search</option>
        <option value="16">Translate</option>
        <option value="21">YouTube</option>
        <option value="6">Gmail</option>
        <!-- Add more products as needed -->
      </select>
      <button type="submit">Update Chart</button>
    </form>
  </div>
</template>
  
  <script>
  import { Line } from 'vue-chartjs'
  import {
    Chart as ChartJS,
    CategoryScale,
    LinearScale,
    PointElement,
    LineElement,
    Title,
    Tooltip,
    Legend,
    Filler,
  } from 'chart.js'
  
  ChartJS.register(
    CategoryScale,
    LinearScale,
    PointElement,
    LineElement,
    Title,
    Tooltip,
    Legend,
    Filler,
    )
  
  export default {
    name: 'LineChart',
    components: { Line },
    data: () => ({
        loaded: false,
        chartData: null,
        startDate:'12-Mar-2023',
        endDate:'15-Mar-2023',
        region:'ZW',
        product:'21',
        chartOptions: {
          responsive: true,
          maintainAspectRatio: false,
          elements: {
            line: {
              borderDash: []
            }
          }
      }
    }),
    async mounted () {
      this.updateData()
    },

  methods: {
    async updateData(){
      console.log("function update data")
      this.loaded = false

    try {
      const start = new Date(this.startDate).getTime();
      const end = new Date(this.endDate).getTime();
      const region = this.region;
      const product = this.product;
      const url = 'https://transparencyreport.google.com/transparencyreport/api/v3/traffic/fraction?start='+start+'&end='+end+'&region='+region+'&product='+product;
      const corsProxyUrl = 'https://cors-anywhere.herokuapp.com/';

      const res = await fetch(corsProxyUrl+url)
      let text = await res.text();
      if (text.startsWith(")]}'\n")) {
        text = text.substring(")]}'\n".length);
      }
      let data = JSON.parse(text);
      // all arrays
      data = data[0][1]
      let vis = {};
      data = data.map((item) => {
        let d = new Date(item[0])
        let dd = d.getDate()
        let mm = d.getMonth()+1
        let yyyy = d.getFullYear()
        d = dd+'/'+mm+'/'+yyyy
        if(vis[d]){
          d = '';
        }else{
          vis[d] = true;
        }
        return {
          date: d,
          value: item[1][0][1]
        }
      })
      console.log(data)
      this.chartData = {
        labels: data.map((item) => item.date),
        datasets: [
          {
            label: "Google Transparency Report",
            backgroundColor: "rgba(66, 133, 244, 0.2)",
            borderColor: "rgba(66, 133, 244, 1)",
            pointBorderColor: "#fff",
            data: data.map((item) => item.value),
            fill: true
          }
        ]
      }
      this.loaded = true
    } catch (e) {
      console.error(e)
    }
    }
  }
  }
  </script>