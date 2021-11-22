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
            <h2>Burgers</h2>
            <p>Choose your burgers here:</p>
          </div>

          <div class="wrapper">
          <Burger v-for="burger in burgers"
                      v-bind:burger="burger"
                      v-bind:key="burger.name"
                      v-on:orderedBurger="addToOrder($event)"/>
          </div>
          </section>



      <section id="Kundinformation" >
        <h3>Customer information</h3>
        <h4>This is where you provide necessary infromation</h4>

        <p>
          <label for="fullname">Full name</label><br>
          <input type="text" id="fullname" v-model="fn" required="required" placeholder="First- and Last name">

        </p>
        <label for="email">Email</label><br>
        <input type="email" id="email" v-model="ema" required="required" placeholder="E-mail address">

        <p>Payment options</p>
        <label for="recipient">Recipient</label><br>
        <select id="recipient" v-model="rcp">
          <option selected="selected">Kort</option>
          <option>Swish</option>
          <option>Kontanter</option>
          <option>Faktura</option>
        </select>

        <p>Gender</p>

        <div>
          <input type="radio" id="Woman" v-model="kon" value="Woman" checked="checked">
          <label for="huey">Woman</label>
        </div>

        <div>
          <input type="radio" id="Man" v-model="kon" value="Man">
          <label for="Man">Man</label>
        </div>

        <div>
          <input type="radio" id="Non Bianary" v-model="kon" value="Non Bianary">
          <label for="Non Bianary">Non Bianary</label>
        </div>
          <p>Destination</p>
        <div id="mapscroll">
        <div id="map" v-on:click="setLocation">
              <div v-bind:style="{left:this.location.x +'px', top:this.location.y+'px'}">
                T
              </div>
          click here to choose your destination
      </div>
      </div>

      </section>
    </main>

    <hr>
    <footer>
    </footer>


  <button type="submit" v-on:click="sendOrder">
    <img src="img/knapp.png">

  </button>
</template>

<script>
import menu from '../assets/menu.json'
import Burger from '../components/Burger.vue'
import io from 'socket.io-client'

const socket = io();

/*function MenuItem(name, gluten, lactose, kCal, picture) {
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
*/




export default {
  name: 'Home',
  components: {
    Burger
  },
  data: function () {
    return {
      burgers:menu,
      fn:'',
      ema:'',
      rcp:'Swish',
      kon:'Woman',
      location:{x:0,y:0},
      orderedBurgers:{}
  }
},
  methods: {
    getOrderNumber: function () {
      return Math.floor(Math.random()*100000);
    },

    sendOrder: function(){
      /*console.log(this.fn,this.ema,this.rcp,this.kon, this.orderedBurgers)*/
      socket.emit("addOrder", { orderId: this.getOrderNumber(),
        details: { x: this.location.x,
          y: this.location.y },
          orderItems: this.orderedBurgers,

          custinf: {name:this.fn,
          email:this.ema, payment:this.rcp, gender:this.kon}

        },
      );
},


    addToOrder: function (event) {
  this.orderedBurgers[event.name] = event.amount;
},

  /*  addOrder: function (event) {
      var offset = {x: event.currentTarget.getBoundingClientRect().left,
        y: event.currentTarget.getBoundingClientRect().top};
        socket.emit("addOrder", { orderId: this.getOrderNumber(),
          details: { x: event.clientX - 10 - offset.x,
            y: event.clientY - 10 - offset.y },
            orderItems: ["Beans", "Curry"]
          },
        );
       this.location.x=event.clientX-10-offset.x
       this.location.y=event.clientY-10-offset.y


     },
     */

     setLocation: function (event) {
       var offset = {x: event.currentTarget.getBoundingClientRect().left,
         y: event.currentTarget.getBoundingClientRect().top};
        this.location.x=event.clientX-10-offset.x
        this.location.y=event.clientY-10-offset.y
      },
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
      position: relative;
      margin: 0;
      padding: 0;
      background: url(/img/polacks.jpg);
      background-repeat: no-repeat;
      width:1920px;
      height: 1078px;
      cursor: crosshair;
    }

    #map div {
      position:absolute;
      background: black;
      color: white;
      border-radius: 10px;
      width:20px;
      height:20px;
      text-align: center;
    }


  #mapscroll{
    overflow:scroll;
    width: 400px;
    height: 400px;
  }


  </style>
