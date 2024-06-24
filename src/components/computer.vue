<template>
<div class="item">
  <b-icon-pc-display-horizontal class="computer"/>
  <b>{{ item.ip }}
   
    <span v-if="item.online" class="online"></span>
    <span v-else class="offline"></span>
  </b>

</div>
</template>

<script>
export default {
  props:[ "item" ],
  data(){
    return {
    }
  },
}
</script>
<style scoped>
.item{
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 10px;
}
.computer{
  font-size: 5em;
  box-shadow: 2px 2px 5px;
  padding: 10px;
}

.online, .offline{
  width: 20px;
  height: 20px;
  border-radius: 50%;
  display: inline-block;
}
.online{
  background-color: #0f0;
}

.offline{
  background-color: red;
}
</style>
