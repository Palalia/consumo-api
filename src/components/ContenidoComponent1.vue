<template>
    <v-app> 
        <v-select
          v-model="select"
          :items="ItemGroups"
          item-text="ItmsGrpNam"
          item-value="ItmsGrpNam"
          label="Selecciona una opccion"
          multiple
        
          return-object
        ></v-select>
    </v-app>    
</template>
  <script>
  import axios from 'axios';
    export default {
      data:()=>({
        favorites: [],
          states: [
            'Alabama', 'Alaska', 'American Samoa', 'Arizona',
            'Arkansas', 'California', 'Colorado', 'Connecticut',
            'Delaware', 'District of Columbia', 'Federated States of Micronesia',
            'Florida', 'Georgia', 'Guam', 'Hawaii', 'Idaho',
            'Illinois', 'Indiana', 'Iowa', 'Kansas', 'Kentucky',
            'Louisiana', 'Maine', 'Marshall Islands', 'Maryland',
            'Massachusetts', 'Michigan', 'Minnesota', 'Mississippi',
            'Missouri', 'Montana', 'Nebraska', 'Nevada',
            'New Hampshire', 'New Jersey', 'New Mexico', 'New York',
            'North Carolina', 'North Dakota', 'Northern Mariana Islands', 'Ohio',
            'Oklahoma', 'Oregon', 'Palau', 'Pennsylvania', 'Puerto Rico',
            'Rhode Island', 'South Carolina', 'South Dakota', 'Tennessee',
            'Texas', 'Utah', 'Vermont', 'Virgin Island', 'Virginia',
            'Washington', 'West Virginia', 'Wisconsin', 'Wyoming',
          ],
          ItemGroups:[],
      }),
      mounted(){
        axios.get('http://sap.playerytees.com:8091/api/test/ItemGroups')
        .then(response => {
           console.log((response.data.data["0"]["ItmsGrpCod"]));
            response.data.data.forEach(elemento=>(this.ItemGroups.push(elemento["ItmsGrpNam"]+" "+elemento["ItmsGrpDesc"])));
            console.log(this.ItemGroups);
            /*
            response.data.data.forEach(aux=>{
                this.ItemGroups.push(aux["ItmsGrpCod"])
                }
            );*/
            
        })
    }
    }
  </script>