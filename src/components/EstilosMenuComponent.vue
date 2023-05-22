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
        <v-simple-table  v-for="(modelo,index) in ItemsColor" :key="index"  >
          <template v-slot:default>
            <thead>
              <tr>
                <th class="text-left">
                  Estilo
                </th>
                <th class="text-left">
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
                  <tr v-for="(item,index) in modelo" :key="index">
                    <td>{{item.estilo}}</td>
                    <td>{{item.descripcion}}</td>
                    <td><input type="color" class="form-control form-control-color rounded-5" disabled  :value="item.color"></td>
                    <td>{{item.s}}</td>
                    <td>{{item.M}}</td>
                    <td>{{item.L}}</td>
                    <td>{{item.XL}}</td>
                    <td>{{item.XL2}}</td>
                    <td>{{item.XL3}}</td>
                    <td>{{item.XL4}}</td>
                    <td>{{item.XL5}}</td>
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
              this.Items[i]=response.data.data;
              arraux.forEach(elemento=>(
              this.colores.push({
                title:elemento["ColorDesc"],
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
    LlenarTablas(colores){
      //var arraux=[];
          for(var i=0;i<this.Items.length;i++){
            for(var j=0;j<colores.length;j++){
              var cadena=[{
                  id:"",
                  estilo:"",
                  color:"",
                  descripcion:"",
                  s:"0",
                  M:"0",
                  L:"0",
                  XL:"0",
                  XL2:"0",
                  XL3:"0",
                  XL4:"0",
                  XL5:"0"
              }];
              if(this.ItemsColor[i] != undefined){
               
                let aux=this.Items[i].filter(function(elemento){
                              return elemento.Color==colores[j];
                            });
                            
                            aux.forEach((elemento) => {
                    cadena[0].id=j;
                    cadena[0].estilo=elemento.ItmsGrpNam;
                    cadena[0].color=elemento.ColorHex;
                    
                    cadena[0].descripcion=elemento.ItemName;
                    switch(elemento.Talla){
                      case "s":
                        cadena[0].s=elemento.Stock;
                      break;
                      case "M":
                      cadena[0].M=elemento.Stock;
                      break;
                      case "L":
                      cadena[0].L=elemento.Stock;
                      break;
                      case "XL":
                      cadena[0].XL=elemento.Stock;
                      break;
                      case "2XL":
                      cadena.XL2=elemento.Stock;
                      break;
                      case "3xl":
                      cadena[0].XL3=elemento.Stock;
                      break;
                      case "4XL":
                      cadena[0].XL4=elemento.Stock;
                      break;
                      case "5xl":
                      cadena[0].XL5=elemento.Stock;
                      break;

                    }
                  });
                let suma=cadena[0].s+cadena[0].L+cadena[0].M+cadena[0].XL+cadena[0].XL2+cadena[0].XL3+cadena[0].XL4+cadena[0].XL5;
                if(suma>0){
                  let final=[this.ItemsColor[i],cadena].flat();
                  this.ItemsColor[i]=final;
                }  
                
              }else{
               
                var aux=this.Items[i].filter(function(elemento){
                              return elemento.Color==colores[j];
                            });
                if(aux.length>0){
                  //this.ItemsColor[i]=aux;
                  aux.forEach((elemento) => {
                    cadena[0].estilo=elemento.ItmsGrpNam;
                    cadena[0].color=elemento.ColorHex;
                    cadena[0].descripcion=elemento.ItemName;
                    switch(elemento.Talla){
                      case "S":
                        cadena[0].s=elemento.Stock;
                      break;
                      case "M":
                      cadena[0].M=elemento.Stock;
                      break;
                      case "L":
                      cadena[0].L=elemento.Stock;
                      break;
                      case "XL":
                      cadena[0].XL=elemento.Stock;
                      break;
                      case "2XL":
                      cadena[0].XL2=elemento.Stock;
                      break;
                      case "3xl":
                      cadena[0].XL3=elemento.Stock;
                      break;
                      case "4XL":
                      cadena[0].XL4=elemento.Stock;
                      break;
                      case "5xl":
                      cadena[0].XL5=elemento.Stock;
                      break;

                    }
                  });
                let suma=cadena[0].s+cadena[0].L+cadena[0].M+cadena[0].XL+cadena[0].XL2+cadena[0].XL3+cadena[0].XL4+cadena[0].XL5;
                if(suma>0)
                this.ItemsColor[i]=cadena;  
              }

            }
            }            
          }
        //var very=[];
        this.ItemsColor.forEach(e=>{
          //if(e=>)
          console.log(e.length);
        });
     },
    changeColorState(colores){
          //this.ItemsColor=[];
          this.DibuarTablas=false;
          this.ItemsColor=[];
           if(colores!=""){
            this.LlenarTablas(colores);
            
          }else{
            //this.colores=[]
          }
          this.DibuarTablas=true;
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