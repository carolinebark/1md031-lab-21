<template>


    <header>
      <div id="header">
        <img id="bild" src="img/02.jpg">
        <h1 id="rubrik">Välkommen till CarroBarros Burgare</h1>
      </div>
    </header>
    <main>

       <section id="hamburgare">
          <div class="välj">
            <h2>Hamburgare</h2>
            <p>Välj din burgare nedan</p>
          </div>
          <div class="wrapper">
          <Burger v-for="burger in burgers"
                      v-bind:burger="burger"
                      v-bind:key="burger.name"/>

<!--
          <div class="burger1">
            <h2>Carros special</h2>
            <img src="img/lule.jpg"  alt="Span"  style="height:200px;">
            <ul>
              <li>Innehåller <span class="ingredienser"> Laktos </span> </li>
              <li>Pris <span class="ingredienser"> 135 kr </span> </li>
            </ul>
          </div>


          <div class="burger2">
            <h2>Barros special</h2>
            <img src="img/burger2.jpg" alt="Span"  style="height: 200px;">
            <ul>
              <li>Innehåller <span class="ingredienser"> Gluten</span> </li>
              <li>Pris <span class="ingredienser"> 105 kr </span> </li>
            </ul>
          </div>



          <div class="burger3">
            <h2>CarroBarros special med extra allt</h2>
            <img src="img/extra.jpg" alt="Span" style="height: 200px;" >
            <ul>
              <li>Innehåller <span class="ingredienser"> Gluten, Laktos</span> </li>
              <li>Pris <span class="ingredienser"> 150 kr </span> </li>
            </ul>
          </div>
        -->
        </div>

      </section>



      <section id="Kundinformation" >
        <h3>Kundinformation</h3>
        <p>Här uppger anger du nödvändig information</p>
        <h4>Leveransinformation</h4>
        <p>Fyll i dina uppgifter nedan</p>
        <p>
          <label for="fullname">Full name</label><br>
          <input type="text" id="fullname" name="fn" required="required" placeholder="First- and Last name">
        </p>
        <label for="email">Email</label><br>
        <input type="email" id="email" name="em" required="required" placeholder="E-mail address">
        <p>
          <label for="street">Street</label><br>
          <input type="text" id="street" name="ln" placeholder="Street name">
        </p>
        <p>
          <label for="house">House</label><br>
          <input type="number" id="house" name="em" required="required" placeholder="House number">
        </p>
        <p>Payment options</p>
        <label for="recipient">Recipient</label><br>
        <select id="recipient" name="rcp">
          <option selected="selected">Kort</option>
          <option>Swish</option>
          <option>Kontanter</option>
          <option>Faktura</option>
        </select>
        <p>Gender</p>

        <div>
          <input type="radio" id="Woman" name="kon" value="Woman" checked="checked">

          <label for="huey">Woman</label>
        </div>

        <div>
          <input type="radio" id="Man" name="kon" value="Man">
          <label for="Man">Man</label>
        </div>

        <div>
          <input type="radio" id="Non Bianary" name="kon" value="Non Bianary">
          <label for="Non Bianary">Non Bianary</label>
        </div>
      </section>
    </main>

    <button type="submit">
      <img src="img/knapp.png">
    </button>
    <hr>
    <footer>
    </footer>
    <div id="map" v-on:click="addOrder">
      click here
  </div>
</template>

<script>
import Burger from '../components/Burger.vue'
import io from 'socket.io-client'

const socket = io();

function MenuItem(name, gluten, lactose, kCal, picture) {
  this.Name = name;
  this.Gluten =gluten;
  this.Lactose=lactose;
  this.kCal= kCal;
  this.Picture = picture;
}

const burger1= new MenuItem('Carros special',false,true,1547,"https://media.istockphoto.com/photos/favourite-burger-toppings-picture-id469660542?k=20&m=469660542&s=612x612&w=0&h=1LfG2K-CgB78QgcXJllD8riD3WyPLFUfR0tfihlEmhw=");
const burger2= new MenuItem('Barros special',true, false,3490,"https://img.koket.se/standard-mega/tommy-myllymakis-saftiga-cheeseburgare.jpg");
const burger3= new MenuItem('CarroBarros special',true, true,3467,"https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQ9eIpvvHtJ6XJVBhwc_TH_riJfiUlLKnO_9w&usqp=CAU");

const allBurgers=[burger1,burger2,burger3];
console.log(allBurgers);




export default {
  name: 'Home',
  components: {
    Burger
  },
  data: function () {
    return {
      burgers:allBurgers

    }
  },
  methods: {
    getOrderNumber: function () {
      return Math.floor(Math.random()*100000);
    },
    addOrder: function (event) {
      var offset = {x: event.currentTarget.getBoundingClientRect().left,
        y: event.currentTarget.getBoundingClientRect().top};
        socket.emit("addOrder", { orderId: this.getOrderNumber(),
          details: { x: event.clientX - 10 - offset.x,
            y: event.clientY - 10 - offset.y },
            orderItems: ["Beans", "Curry"]
          }
        );
      }
    }
  }
  </script>

  <style>
  @import url("https://fonts.googleapis.com/css?family=Droid+Serif|Share+Tech+Mono");
  body {
    font-family: arial, sans-serif;
    font-size: 1em;
    font-style: italic;
  }
  .ingredienser{
    font-weight: bold;
  }


  #hamburgare {
    background-color: black;
    color: white;
    border:2px dotted white;
    border-radius:20px;
    margin-right:20px;
  }

  #Kundinformation{
    background-color: white;
    color: black;
    border:2px dotted black;
    border-radius:20px;
  }

  #header{
    overflow: hidden;
    width: 100%;
    height: auto;

  }
  #rubrik{
    position: absolute;
    margin-top: -100px;
  }

  #bild{
    width: 100%;
    height: 200px;
    opacity:0.8;
  }

  .wrapper{
    grid-template-columns: 350px 350px 350px;
    display:grid;
    grid-gap:20px;
  }

  /*.välj{
    grid-column: 1 /span 3;
    grid-row: 1;
    margin: 10px;

  }
  */

  /*.burger1{
    grid-column: 1;
    grid-row:2;
    margin: 10px;

  }

  .burger2{
    grid-column: 2;
    grid-row:2;
    margin: 10px;
  }

  .burger3{
    grid-column: 3;
    grid-row:2;
    margin: 10px;
  }
  */

  button:hover {
    background-color: blue;
    cursor:pointer;
  }

  section{
    margin-left:20px;
    margin-right:20px;
  }
  button{
    margin:50px;

  }

  #map {
    width: 300px;
    height: 300px;
    background-color: red;
  }
  </style>
