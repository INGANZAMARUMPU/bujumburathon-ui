<template>
    <div>
        
        <div class="display_header">
            <h1>Micro Vm Liste</h1>
            <button @click="createMc">
                <b-icon-plus style="size: 24px;" />
                Creer un Micro VM</button>
        </div>
        <table class="table">
            <thead>
                <tr>
                    <th>#</th>
                    <th>Name</th>
                    <th>Ip Adress</th>
                    <th>Date de Creation</th>
                    <th>Status</th>
                    <th>Action</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="item in items" :key="item.id">
                    <td>{{ item.id }}</td>
                    <td>{{ item.nom }}</td>
                    <td>{{ item.ip }}</td>
                    <td>
                        {{  item.date_creation}}
                    </td>
                    <td>
                        {{ item.status == 1? "Online" : "Offline" }}
                    </td>
                    <td>
                        <button class="btn btn-primary" @click="startPc(item.id)">Start</button>
                      
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
                alert("ok");
            }
        }
    }
</script>

<style  scoped>

.display_header{
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 10px 0;
}

</style>