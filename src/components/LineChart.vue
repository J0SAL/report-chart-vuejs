<template>
    <Line
      id="my-chart-id"
      :options="chartOptions"
      :data="chartData"
      v-if="loaded"
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
    Legend 
  } from 'chart.js'
  
  ChartJS.register(
    CategoryScale,
    LinearScale,
    PointElement,
    LineElement,
    Title,
    Tooltip,
    Legend
    )
  
  export default {
    name: 'LineChart',
    components: { Line },
    data() {
      return {
        chartData: {
            labels: ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"],
            datasets: [
                {
                    label: "Google Transparency Report",
                    backgroundColor: "#f87979",
                    data: [10, 20, 30, 40, 50, 60, 70, 80, 90, 100, 10, 20] 
                } 
            ]
        },
        chartOptions: {
          responsive: true,
          maintainAspectRatio: false
        }
      }
    }
  }
  </script>