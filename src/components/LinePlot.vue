<template>
     <Plotly 
         :data="data"
         :layout="layout"
     ></Plotly>
</template>

<script>
import { Plotly } from 'vue-plotly'

export default {
 components: {
   Plotly
 },
 data () {
     return {
       data: [{
           x: [1,2,3,4],
           y: [10,15,13,17],
           type:"scatter"
         }],
       layout:{
           title: "My graph",
           showlegend: true,
       },
     }
 }
}
</script>