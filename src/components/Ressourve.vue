<template>
    <div class="parent">
      <div class="title inline">
        <h4>Utilisation des resources</h4>
        <More/>
      </div>
      <div class="chart">
        <Doughnut :data="chart_data" :options="chart_options"/>
      </div>
    </div>
  </template>
  <script>
  import { Doughnut } from 'vue-chartjs';
  
  import More from "./more"
  
  export default {
    props:[
      "data"
    ],
    components:{
      More,
      Doughnut
    },
    data(){
      return {
        chart_options: {
          cutout: 60,
          plugins:{
            legend:{
              position: "left",
              align: "right",
              fullWidth: true,
              labels: {
                boxWidth: 10,
                fontSize: 14,
                pointStyle: 'circle',
                usePointStyle: true
              }
            }
          }
        }
      }
    },
    computed:{
      chart_data(){
        let colors = ['#F07521', '#1DBC1A', '#1A7CBC']
        return {
          labels:Object.keys(this.data),
          datasets:[{
            data:Object.values(this.data),
            backgroundColor:colors
          }]
        }
      }
    }
  };
  </script>
  <style scoped>
  .parent{
    background-color: white;
    border-radius: 10px;
    padding: 10px;
    font-size: .8em;
  }
  .parent>*{
    margin: 5px;
  }
  .amount{
    font-weight: 600;
    margin-left: 5px;
  }
  .inline{
    display: flex;
    align-items: center;
  }
  .title{
    display: flex;
    justify-content: space-between;
  }
  .chart{
    display: flex;
    justify-content: center;
    align-items: center;
  }
  .chart>*{
    max-width: 280px;
    max-height: 280px;
  }
  </style>