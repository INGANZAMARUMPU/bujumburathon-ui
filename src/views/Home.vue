<template>
  <div id="app">
    <TopBar @create="button_shown=true"/>
    <div class="body">
      <div class="body_items">
        <img src="/static/ant.webp">
        <div class="camera">
          <div class="top">
            <div v-if="!!msg">
              <div>new command: <strong>{{ msg }}</strong></div>
            </div>
          </div>
          <div class="computers">
            <div v-if="isLoading" class="loadingComputers">
              Loading
              <div class="loader"></div>
            </div>
            <div    class="computers">
              <Computer v-for="computer in computers" :item="computer" 
              @click="displayInfoPc(computer)" :key="computer.id"/>
            </div>
           
          </div>
        </div>
        <div class="main-screen ">
          <div>
            <div class="info_pc" v-if="current_pc.ip">
              <h4>Information detail du Machine : {{ current_pc.ip }}
                Serveur No : {{ current_pc.id }}
              </h4>
            </div>
            <div class="computer_detail" v-if="!isLoading">
              <Ressource
              v-for="obj,i in rsrcs"
              :rsrc_name="i"
              :rsrc_obj="obj"
              :key="obj.id"
              />
            </div>
          </div>
          <div class="microvms">
            <micro-vm-liste :items="microvms" @created="updateMicroVm"/>
          </div>
        </div>
      </div>
      
      <DialogButton
      :item="current_button"
      @close="closeDialogs"
      v-show="button_shown"/>
    </div>
    
  </div>
</template>

<script>
import TopBar from '@/components/topbar.vue'
import Computer from '@/components/computer.vue'
import DialogButton from '@/components/dialog_button.vue'
import MicroVmListe from '@/components/MicroVmListe.vue'
import Ressource from '@/components/ressource.vue'
import axios from 'axios'

export default {
  components: {
    TopBar, Computer, DialogButton, MicroVmListe, Ressource
  },
  data(){
    return {
      rsrcs:{},
      interval:null,
      selected:{},
      current_pc:{},
      selected_computer:{},
      computers:[],
      microvms:[],
      isLoading: false,
      buttons:this.$store.state.buttons,
      msg:"",
      button_shown: false,
      current_button: {},
      rsrcs:{},
      interval:null,
    }
  },
  watch:{
    "$store.state.user"(new_val){
      this.fetch_data()
    },
  },
  methods:{
    updateMicroVm(e){
      alert("Finish")
      //this.microvms.push(e);
      this.fetch_data()
    },
    editButton(button){
      this.current_button = button
      this.button_shown = true
    },
    deleteButton(index){
      this.$store.state.buttons.splice(index, 1)
    },
    displayInfoPc(computer){
      this.current_pc = computer
      this.isLoading = true;
      this.$store.state.current_pc =computer
      let url = `http://${computer.ip}:8000/micro_vms/resources_monitor/`
      let vue = this
      if(!!this.interval){
        window.clearInterval(this.interval)
      }
      this.interval = setInterval(() => {
        axios.get(url).then((response) => {
          vue.rsrcs = response.data

          this.isLoading = false;
        }).catch((error) => {
          console.error(error)
        })
      }, 6000)
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
.info_pc{
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  background-color: rgb(0, 0, 0);
  color: aliceblue;
}

.loadingComputers{
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  z-index: 10;
}
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
  height: calc(100% - 200px);
}

.body_items{
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  width: 100%;
  height: 100%;
  overflow: auto;
}
.camera{
  position: absolute;
  height: 170px;
  top: 60px;
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
.computer_detail{
  display: grid;
  grid-template-columns: repeat(4, 1fr);
}

.microvms{
  position: absolute;
  top: 240px;
  right: 0;
  height: calc(100% - 240px);
  overflow: auto;
}
.computer_detail{
  position: absolute;
  top: 270px;
  left: 0;
  height: calc(100% - 270px);
  overflow: auto;
}

.info_pc{
  position: absolute;
  top: 240px;
  left: 0;
  height:20px;
  overflow: auto;
}

.loader {
  width: 0;
  height: 50px;
  display: inline-block;
  position: relative;
  background: #FFF;
  box-shadow: 0 0 20px rgba(255, 255, 255, 0.5);
  box-sizing: border-box;
  animation: animFw 8s linear infinite;
}
  .loader::after,
  .loader::before {
    content: '';
    width: 10px;
    height: 1px;
    background: #FFF;
    position: absolute;
    top: 9px;
    right: -2px;
    opacity: 0;
    transform: rotate(-45deg) translateX(0px);
    box-sizing: border-box;
    animation: coli1 0.3s linear infinite;
  }
  .loader::before {
    top: -4px;
    transform: rotate(45deg);
    animation: coli2 0.3s linear infinite;
  }

@keyframes animFw {
    0% {
  width: 0;
}
    100% {
  width: 100%;
}
  }

@keyframes coli1 {
    0% {
  transform: rotate(-45deg) translateX(0px);
  opacity: 0.7;
}
    100% {
  transform: rotate(-45deg) translateX(-45px);
  opacity: 0;
}
  }

@keyframes coli2 {
    0% {
  transform: rotate(45deg) translateX(0px);
  opacity: 1;
}
    100% {
  transform: rotate(45deg) translateX(-45px);
  opacity: 0.7;
}
  }
</style>
