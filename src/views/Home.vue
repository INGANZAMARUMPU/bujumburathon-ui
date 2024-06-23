<template>
  <div id="app">
    <TopBar @create="button_shown=true"/>
    <div class="body">
      <img src="/static/ant.webp">
      <div class="camera">
        <div class="top">
          <div class="title">{{ selected.name||'*hitamwo irobot*' }}</div>
          <div v-if="!!msg">
            <div>new command: <strong>{{ msg }}</strong></div>
          </div>
        </div>
        <div class="robots">
          <Computer
            v-for="ant in robots" :robot="ant"
            :selected="selected.id==ant.id"
            @selected="switchTo(ant)"
            :key="ant.id"/>
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
import Computer from '@/components/robot.vue'
import DialogButton from '@/components/dialog_button.vue'

export default {
  components: {
    TopBar, Computer, DialogButton
  },
  data(){
    return {
      selected:{},
      robots:[],
      buttons:this.$store.state.buttons,
      msg:"",
      button_shown: false,
      current_button: {}
    }
  },
  watch:{
    "$store.state.buttons"(new_val){
      this.buttons = new_val
    },
  },
  methods:{
    editButton(button){
      this.current_button = button
      this.button_shown = true
    },
    deleteButton(index){
      this.$store.state.buttons.splice(index, 1)
    },
  },
  mounted(){
  }
}
</script>
<style scoped>
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
.robots{
  display: flex;
  flex-direction: row-reverse;
  flex-wrap: wrap-reverse;
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
