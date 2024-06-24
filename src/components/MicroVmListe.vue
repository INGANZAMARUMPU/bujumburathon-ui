<template>
    <div>
        <div class="display_header">
            <h1>Micro Vm Liste</h1>
            <button @click="createMc">
                <b-icon-plus style="size: 24px;" />
                Creer un Micro VM</button>
        </div>
        <div v-if="loader">
            <h1>Loading</h1>
        </div>
        <table class="table">
            <thead>
                <tr>
                    <th>#</th>
                    <th>Name</th>
                    <th>Ip Adress</th>
                    <th>Machine Host</th>
                    <th>Date de Creation</th>
                    <th>Status</th>
                    <th>Action</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="item in items" :key="item.id">
                    <td>{{ item.id }}</td>
                    <td>{{ item.nom }}</td>
                    <td>
                        <b-icon-pc-display/>
                        {{ item.ip }}</td>
                    <td>
                        <b-icon-pc/>
                        {{ item.serveur.ip }}
                        <span v-if="item.serveur.online" class="online"></span>
                        <span v-else class="offline"></span>
                    </td>
                    <td>
                        {{  item.date_creation}}
                    </td>
                    <td>
                        <span v-if="item.serveur.online" class="online"></span>
                        <span v-else class="offline"></span>
                        <!-- {{ item.status == 1? "Online" : "Offline" }} -->
                    </td>
                    <td>
                        <div class="">
                            <button class="btn btn-primary" @click="startPc(item.id)">Start</button>
                            <button class="btn btn-primary" @click="deletePc(item.id)">Delete</button>
                        </div>
                    </td>
                </tr>
            </tbody>
        </table>
    </div>
</template>

<script>
    export default {
        props:["items"],
        data(){
            return {
                loader:false
            }
        },
        methods:{
            startPc(pcId){
                alert(pcId);
                //axios.get()
            }, 
            createMc(){
                const machneName = prompt("Entrez le nom de la machine");
                if(machneName && !!this.$store.state.current_pc.id){
                    this.loader = true;
                    axios.post(this.url + '/micro-vms/',{
                        "nom":machneName,
                        "serveur":this.$store.state.current_pc.id
                    } , this.header)
                    .then(response=>{
                        console.log(response);
                        this.$emit('created',  response.data)
                       
                    }).catch(error=>{
                        console.log(error);
                    })
                    .finally(()=>{
                        this.loader = false;
                    })
                }else{
                    alert("Veuillez choisir une machine");
                }
            }
        }
    }
</script>

<style  scoped>

.display_header{
    position: obsolute;
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 10px 0;
}


</style>