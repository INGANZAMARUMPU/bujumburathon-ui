<template>
  <div class="parent">
    <div class="title inline">
      <h4 :class="vpn_status">{{rsrc_name}}</h4>
    </div>
    <div class="inline" >
      <div class="sm"><strong>Used : {{getPercent}} %</strong></div>
    </div>
     <div class="chart">
      <Doughnut :data="chart_data" :options="chart_options"/>
    </div>
  </div>
</template>
<script>
import { Doughnut } from 'vue-chartjs';
export default {
  data(){
    return {
      obj: this.rsrc_obj,
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
  props:["rsrc_name", "rsrc_obj"],
  components:{Doughnut},
  computed: {
    vpn_status() {
      if(this.rsrc_obj?.state=="ON")
        return "status-on"
      else
        return "status-off"
    },
    chart_data(){
      let colors = ['#F07521', '#1DBC1A']
      return {
        labels:['Used', "Free"],
        datasets:[{
          data:[this.rsrc_obj.used,this.rsrc_obj.total-this.rsrc_obj.used],
          backgroundColor:colors
        }]
      }
    },
    getPercent(){
      return (this.rsrc_obj.used*100/this.rsrc_obj.total).toFixed(1)
    }
  },
  methods: {
  },
};
</script>
<style scoped>
.parent{
  background-color: white;
  border-radius: 10px;
  padding: 10px;
  min-width: 240px;
}
.parent>*{
  margin: 5px;
}

.inline{
  display: flex;
  align-items: center;
}

.chart{
  display: flex;
  justify-content: center;
  align-items: center;
}
.chart>*{
  max-width: 300px;
  max-height: 300px;
}

</style>
