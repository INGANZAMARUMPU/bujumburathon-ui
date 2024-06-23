<template>
    <div>
        <table>
            <thead></thead>
        </table>
    </div>
</template>

<script>
    export default {
       data(){
        return{

        }
       }, 
       mounted(){
        this.get();
       },
       methods:{
        get(url){
            axios.get()
        }
       }, 
       computed:{
        currentUrl(){
            return this.$store.state.url + '';
        }
       }
    }
</script>

<style lang="scss" scoped>

</style>