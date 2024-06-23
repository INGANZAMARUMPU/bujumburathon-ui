<template>
    <div>
        <h1>Liste des Machines  <BIconArrow90degDown />
            <!-- Or -->
            <b-icon-arrow-90deg-down /></h1>
        <table>
            <thead>
                <tr>
                    <th>#</th>
                    <th>Nom</th>
                    <th>Status</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="machine in machines" :key="machine.id">
                    <td>{{machine.id}}</td>
                    <td>{{machine.name}}</td>
                    <td>{{machine.status}}</td>
                </tr>
            </tbody>
        </table>
    </div>
</template>

<script>

export default {
    data(){
        return{
            machines:[]
        }
    },
    methods:{
        fetchAll(){
            axios.get(this.url + "/micro-vms/", this.headers)
            .then(response => (this.machines = response.data))
        }
    },
    mounted(){
        this.fetchAll();
        alert("called"); 
    }
}
</script>

<style lang="scss" scoped>

</style>