<template>
    <v-app> 
    <b-container class="bv-example-row">
      <b-row>
       <b-col><!---Select Estilos-->
          <v-select
          :items="ItemGroups"
          item-text="title"
          item-value="ItmsGrpCod"
          v-bind:label=label
          v-on:input="limiter"
          multiple
          chips
          clearable
          hint="Estilos"
          persistent-hint
          @change="changeState"
          ></v-select>
        </b-col>
        
        <b-col>
        </b-col>      
        
        <b-col><!--select colores-->
          <v-select
          :items="colores"
          item-text="title"
          item-value="Color"
          label="Escoge un color"
          v-on:change="changeColorState"
          multiple
          chips
          clearable
          hint="Colores"
          persistent-hint
          >
          </v-select>
        </b-col> 
      </b-row>
      <b-row>
       <div > 
        <v-simple-table  v-for="(estilo,index) in ItemsColor" :key="index"  >
          <template v-slot:default>
            <thead>
              <tr>
                <th class="text-left">
                  Estilo
                </th>
                <th class="text-left">
                  Color
                </th>
                <th class="text-left">
                  S
                </th>
                <th class="text-left">
                  M
                </th>
                <th class="text-left">
                  L
                </th>
                <th class="text-left">
                  XL
                </th>
                <th class="text-left">
                  2XL
                </th>
                <th class="text-left">
                  3XL
                </th>
                <th class="text-left">
                  4XL
                </th>
                <th class="text-left">
                  5XL
                </th>
              </tr>
              </thead>
              <tbody>
                  <tr v-for="item in estilo" :key="item.name">
                      <td>{{ item.ItmsGrpNam }}</td>
                  <td>{{ item.ColorDesc }}</td>
                </tr>
              </tbody>
          </template>
        </v-simple-table>
       </div> 
      </b-row>
    </b-container>
    </v-app>    
</template>
  <script>
  import axios from 'axios';
    export default {
      data:()=>({
        favorites: [],
        ItemGroups:[],
        colores:[],
        Items:[],
        ItemsColor:[],
        label:"Selecciona un estilo",
        DibuarTablas:false,
      }),
      methods:{
        limiter(e) {
      if(e.length > 5) {
        this.label="Solo puede seleccionar 5 estilos"
        e.pop()
      }
    },
    llenarcolores(valor,i){
      var url='http://sap.playerytees.com:8091/api/test/Items?ItmsGrpCod='+valor;
      axios.request(url)
           .then(response=>{
              var aux={};
              var arraux=response.data.data.filter(function(elemento){
                var exist=!aux[elemento.ColorHex]
                aux[elemento.ColorHex]=true;
                return {exist};
              });
              console.log("tamaño "+response.data.data.length);
              this.Items[i]=response.data.data;
              arraux.forEach(elemento=>(
              this.colores.push({
                title: elemento["ColorHex"]+" "+elemento["ColorDesc"],
                ColorHex:elemento["ColorHex"],
                Color:elemento["Color"]
              })
              
              ));
           })
           .catch(err=>console.log(err));
    },
    changeState(valor){
          if(valor!=""){
            this.colores=[];
            this.ItemsColor=[];
            /////////
            this.Items=[];
            for(let i=0;i<valor.length;i++){
              this.llenarcolores(valor[i],i);
            }
             ///////////// 
          }else{
            this.colores=[];
            this.ItemsColor=[];

          }
    },
    LlenarTablas(colores){/*
          console.log(colores);
          console.log(colores.length);*/
          for(var i=0;i<this.Items.length;i++){
            for(var j=0;j<colores.length;j++)
              if(this.ItemsColor[i] != undefined){
                console.log("no undefined");
                let aux=this.Items[i].filter(function(elemento){
                              return elemento.Color==colores[j];
                            });
                console.log("aux:");
                console.log(aux);
                console.log("itemscolor[i]:");
                console.log(this.ItemsColor[i]);
                let final=[this.ItemsColor[i],aux].flat();
                this.ItemsColor[i]=final;
              }else{
                console.log(" undefined");
                var aux=this.Items[i].filter(function(elemento){
                              return elemento.Color==colores[j];
                            });
                console.log(aux);
                if(aux.length>0){
                  console.log("aux es malloy que 0")
                  this.ItemsColor[i]=aux;
                }
            }            
          }
          console.log(this.ItemsColor);
     },
    changeColorState(colores){
          console.log(colores);
          //this.ItemsColor=[];
          this.DibuarTablas=false;
          this.ItemsColor=[];
          console.log("limpiar items color tam "+this.ItemsColor.length);
          if(colores!=""){
            this.LlenarTablas(colores);
            
          }else{
            //this.colores=[]
          }
          this.DibuarTablas=true;
          console.log("itemscolor tamaño"+this.ItemsColor.length);
          console.log(this.ItemsColor);
    }
    },
      mounted(){
        axios.get('http://sap.playerytees.com:8091/api/test/ItemGroups')
        .then(response => {
            response.data.data.forEach(elemento=>(
                                        this.ItemGroups.push({title: elemento["ItmsGrpNam"]+" "+elemento["ItmsGrpDesc"],
                                                              ItmsGrpCod: elemento["ItmsGrpCod"] })));
        })
      }
    }
  </script>