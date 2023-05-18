<template>
  <div>
  <b-container class="bv-example-row">
    <b-row>
      <b-col>1 of 3</b-col>
      <b-col>2 of 3</b-col>
      <b-col>3 of 3</b-col>
      </b-row>
    </b-container>
    <EstilosMenu></EstilosMenu></div>
</template>
<script>
  import EstilosMenu from './EstilosMenuComponent.vue'
  export default {
    name: 'App',
    components: {
    EstilosMenu
    }
  }
</script>