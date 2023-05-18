<template>
  <div id="app">
    <VueSidebarMenuAkahon  
    :menu-title="'Examen'" 
    :menu-logo='"https://uniformesenqueretaro.com.mx/img/Logo-Playerytees.pngg"'
    :menu-items="[{link: '#',name: 'inventario', tooltip: 'inventario', icon: 'bx-grid-alt'}]"
    :profile-name="'Angel Arce'"
    :profile-role="'Aspirante vacante de Progra'"></VueSidebarMenuAkahon>
    <ContenidoComponent></ContenidoComponent>
   
  </div>
</template>

<script>
import ContenidoComponent from './components/ContenidoComponent.vue'
import VueSidebarMenuAkahon from "vue-sidebar-menu-akahon";
export default {
  name: 'App',
  components: {
  //  HelloWorld
  ContenidoComponent,
  VueSidebarMenuAkahon
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
 /*background-color: #7d7577;*/
}
</style>
