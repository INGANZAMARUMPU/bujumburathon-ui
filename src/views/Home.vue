<template>
  <div id="app">
    <TopBar @create="button_shown=true"/>
    <div class="body">
      <img src="/static/ant.webp">
      <div class="camera">
        <div class="top">
          <div v-if="!!msg">
            <div>new command: <strong>{{ msg }}</strong></div>
          </div>
        </div>
        <div class="computers">
          <Computer v-for="computer in computers" :item="computer" 
          @click="displayInfoPc(computer)" :key="computer.id"/>
        </div>
      </div>
      <div class="main-screen">
        <div class="computer_detail">
          <h1>
            {{ selected_computer }}
          </h1>
        </div>
        <div class="microvms">
          <micro-vm-liste :items="microvms"/>
        </div>

      </div>
      
    </div>
    
    <DialogButton
      :item="current_button"
      @close="closeDialogs"
      v-show="button_shown"/>
  </div>
</template>

<script>
import TopBar from '@/components/topbar.vue'
import Computer from '@/components/computer.vue'
import DialogButton from '@/components/dialog_button.vue'
import MicroVmListe from '@/components/MicroVmListe.vue'
import axios from 'axios'

export default {
  components: {
    TopBar, Computer, DialogButton,
    MicroVmListe
  },
  data(){
    return {
      rsrcs:{},
      interval:null,
      selected:{},
      selected_computer:{},
      computers:[],
      microvms:[],
      buttons:this.$store.state.buttons,
      msg:"",
      button_shown: false,
      current_button: {}
    }
  },
  watch:{
    "$store.state.user"(new_val){
      this.fetch_data()
    },
  },
  methods:{

    fetchRsrc(url){
      //let url = this.url+"/account/statistiques/resources_monitor/"
      let vue = this
      this.interval = setInterval(() => {
          axios.get(url, this.headers)
          .then((response) => {
            vue.rsrcs = response.data
          }).catch((error) => {
            vue.errorOrRefresh(error, vue.fetchRsrc)
          })
        
      }, 2000)
    },
    editButton(button){
      this.current_button = button
      this.button_shown = true
    },
    deleteButton(index){
      this.$store.state.buttons.splice(index, 1)
    },
    displayInfoPc(computer){
      this.interval = setInterval(() => {
        if(this.interval != 0){
          clearInterval(this.interval)
          this.interval = 0;
        }
        // checking status of servers status
        this.fetch_data()
        axios.get(`http://${computer.ip}:8000/micro_vms/resources_monitor/` )
      .then(res => {
        console.log(res.data)
        this.selected_computer = res.data
      }).catch(err => {
        console.error(err);
      })
      }, 3000, computer)
    },
    fetch_data(){
      axios.get(this.url + "/serveurs/", this.headers)
      .then(res => {
        this.computers = res.data
      })
      .catch(err => {
        console.error(err); 
      })
      axios.get(this.url + "/micro-vms/", this.headers)
      .then(res => {
        this.microvms = res.data
      })
      .catch(err => {
        console.error(err); 
      })


    }
  }
}
</script>
<style scoped>

.main-screen{
  display: flex;
  justify-content: space-between;
  flex-direction: row;
  gap: 10px;
  width: 100%;
  height: 100%;
  padding: 5px 20px;
}
.body{
  position: relative;
}
.camera{
  background-color: #59d;
  color: white;
  padding: 20px;
  width: 100%;
  display: flex;
  flex-direction: column;
}
.top{
  flex: 1;
}
.title{
  font-size: 1.2em;
  font-weight: 800;
}
.computers{
  display: flex;
  gap: 5px;
}
.buttons{
  flex-grow: 1;
  max-width: 49%;
}
.control{
  width: 100%;
  display: flex;
  align-items: center;
  flex-direction: column;
}
img{
  position: absolute;
  right: 0;
  bottom: 0;
  z-index: -1;
  opacity: .2;
  transform: rotate(-20deg);
}
</style>
