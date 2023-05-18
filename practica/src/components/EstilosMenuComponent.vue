<template>
<form>
 <div  class="">
  <div class="multiselect ">
    <div class="selectBox " v-on:click="showCheckboxes()">
      <select>
        <option>Select an option</option>
      </select>
      <div class="overSelect"></div>
    </div>
    <div  id="checkboxes">
       <label for="index" v-for="(item,index) in ItemGroups" :key="index">
        <input type="checkbox" id="item.id" >{{item.ItmsGrpNam}}
      </label>
    </div>
  </div>
</div>
</form>
<!--<div>
    <Multiselect
    v-model="value"
      mode="multiple"
      placeholder="Select options"
      :options="ItemGroups.ItmsGrpCod"
    />
  </div>-->
</template>
<script>
import axios from 'axios';
//import Multiselect from '@vueform/multiselect/dist/multiselect.vue2.js'
export default {
    data:()=>({
        value:null,
        expanded:false,
        ItemGroups:[],
        options:['algo','asd','sdfsdf']
    }),
    components: {
 //     Multiselect,
    },
    methods: {
        showCheckboxes() {
            console.log("dentro de funcion")
            
         var checkboxes = document.getElementById("checkboxes");
          if (!this.expanded) {
            checkboxes.style.display = "block";
            this.expanded = true;
          } else {
            checkboxes.style.display = "none";
            this.expanded = false;
          }
        }
    },
    mounted(){
        axios.get('http://sap.playerytees.com:8091/api/test/ItemGroups')
        .then(response => {
           console.log((response.data.data["0"]["ItmsGrpCod"]));
            this.ItemGroups=response.data.data;
            /*
            response.data.data.forEach(aux=>{
                this.ItemGroups.push(aux["ItmsGrpCod"])
                }
            );*/
            
        })
    }
}
</script>
<style scoped>
.multiselect {
  width: 200px;
  
}
.wrapper{
width:200px;
padding:20px;
height: 150px;
}
.selectBox {
  position: relative;
  
}

.selectBox select {
  width: 100%;
  font-weight: bold;
}

.overSelect {
  position: absolute;
  left: 0;
  right: 0;
  top: 0;
  bottom: 0;
  
}

#checkboxes {
  display: none;
  border: 1px #dadada solid;
  
}

#checkboxes label {
  display: block;
}

#checkboxes label:hover {
  background-color: #1e90ff;
}
</style>