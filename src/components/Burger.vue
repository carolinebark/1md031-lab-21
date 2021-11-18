<template>
  <div>
    {{burger.name}}
  </div>

  <div class="burgers">
    <h2>{{burger.Name}}</h2>
    <img class="burgers" v-bind:src="burger.Picture"  alt="Span"  style="height:200px">
    <ul>
      <li>{{burger.kCal}}</li>
        <span v-if="burger.Lactose==true"> <li> Contains <span class="ingredienser"> lactose </span> </li> </span>
        <span v-if="burger.Gluten==true"> <li> Contains <span class="ingredienser"> gluten </span> </li> </span>
    </ul>
  </div>
</template>

<script>
export default {
  name: 'Burger',
  props: {
    burger: Object
  }
}



</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
