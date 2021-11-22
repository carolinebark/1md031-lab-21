<template>
<figure>
  <div class="burgers">
    <h2>{{burger.Name}}</h2>
    <img class="burgers" v-bind:src="burger.Picture"  alt="Span"  style="height:200px">
    <ul>
      <li>{{burger.kCal}} Calories</li>
        <span v-if="burger.Lactose==true"> <li> Contains <span class="ingredienser"> lactose </span> </li> </span>
        <span v-if="burger.Gluten==true"> <li> Contains <span class="ingredienser"> gluten </span> </li> </span>
    </ul>


  <p>Ammount:</p><button type="button" v-on:click="addBurger">
      +
    </button>
    {{ amountOrdered }}
    <button type="button" v-on:click="removeBurger">
      -
    </button>
  </div>

    </figure>

</template>

<script>
export default {
  name: 'Burger',
  props: {
    burger: Object
  },
  data: function () {
    return {
      amountOrdered: 0,

    }
  },
  methods:{
    addBurger: function(){
      this.amountOrdered ++;
      this.$emit('orderedBurger', {name:this.burger.Name,
                                amount: this.amountOrdered
                              }
  );
    },

  removeBurger: function (){
    this.amountOrdered --;
    if (this.amountOrdered<0){
    this.amountOrdered=0
    }


  }
  }
}





</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
