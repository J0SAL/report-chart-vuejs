<template>
    <Line
      id="my-chart-id"
      :options="chartOptions"
      :data="chartData"
      v-if="loaded"
      :style="{width: '100%', height: '60vh',overflow: 'scroll'}"
    />
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
    this.loaded = false

    try {
      const start = '1678492800000';
      const end = '1679270399999';
      const region = 'ZW';
      const product = '21';
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
      // let vis = {};
      data = data.map((item) => {
        let d = new Date(item[0])
        let dd = d.getDate()
        let mm = d.getMonth()+1
        let yyyy = d.getFullYear()
        d = dd+'/'+mm+'/'+yyyy
        // if(vis[d]){
        //   d = '';
        // }else{
        //   vis[d] = true;
        // }
        return {
          date: d,
          value: item[1][0][1]
        }
      })

      this.chartData = {
        labels: data.map((item) => item.date),
        datasets: [
          {
            label: "Google Transparency Report",
            backgroundColor: "rgba(66, 133, 244, 0.2)",
            borderColor: "rgba(66, 133, 244, 1)",
            pointBackgroundColor: "rgba(66, 133, 244, 1)",
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
  </script>