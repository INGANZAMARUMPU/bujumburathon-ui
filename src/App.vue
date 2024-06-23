<template>
  <Home/>
  <DialogMode v-if="!$store.state.user"/>
</template>
<script>
import DialogMode from "@/components/dialog_mode"
import Home from "@/views/Home"
export default {
  components:{
    DialogMode, Home
  },
	watch:{
		"$store.state.user":{
			deep:true,
			handler(new_val){
				if(!!new_val){
					localStorage.setItem('user', JSON.stringify(new_val));
				} else {
					localStorage.removeItem('user')
				}
			}
		}
	},
  mounted(){
      var user = JSON.parse(localStorage.getItem('user'));
      if (user) {
        this.$store.state.user = user;
      }
  }
}
</script>
<style src="./style.css">
</style>
