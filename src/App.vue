<!-- Andrew Sanders Final Project - App component -->
<template>
  <div id="app">
    <NavBar></NavBar>
    <div class="inline-block" style="display: flex; justify-content: center; align-items: center;" >
      <div class="row row-cols-1 row-cols-md-2 g-4" style="display: inline-grid; grid-template-columns: auto auto auto auto; grid-template-rows: auto auto auto; ">
        <CardComponent class="col"  v-for="show in apiData" v-bind:key="show.id" :movie="show" style="margin-left: 30px; margin-right: 30px"></CardComponent>
      </div>
    </div>
    <div class="footer" >
        <div class="container" >
          <RowComp col1="Title" col2="Adult Tickets" col3="Child Tickets" col4="Subtotal" col5="Remove" style="border-bottom: 1px; border-style: solid; border-right: 0px; border-left: 0px; border-top: 0px;"></RowComp>
          <CartComp class="col" id="cart"  v-for="movi in chosenMov" v-bind:key="movi + Math.random()" :movs="movi" style="margin-top: 15px; border-top: 0px; border-bottom: 1px; border-style: dashed; border-right: 0px; border-left: 0px;"></CartComp>
          <RowComp col4="Total" :col5="totals.toString()"></RowComp>
        </div>
    </div>
  </div>
</template> 
<!-- This template is the basis for the whole project, I turned anything needed to repeat into components where necessary -->
<script>
import 'bootstrap';
import 'bootstrap/dist/css/bootstrap.min.css';
import axios from 'axios';
// importing from outside
import RowComp from './RowComp.vue';
import CardComponent from './CardComponent.vue';
import NavBar from './NavBar.vue';
import CartComp from './CartComp.vue';
// importing from the inside 

export default {
  name: 'App',
  components: {
    NavBar,
    CardComponent,
    CartComp,
    RowComp
  },
  data(){
    return{
      movieShows:["movie/now_playing", "movie/popular","movie/upcoming", "movie/top_rated"],
      apiData: '',
      chosenMov: [],
      movAdAmount: [],
      movChAmount: [],
      totals: 0,
    }
    // The movieShows is what helps update the API, as it has the important variables in it, apiData holds the returned data, chosenMov and the two MovAmounts are parallel arrays that tell us what movies have tickets and how many tickets are bought for them, for example if you buy two adult and one kid Avengers ticket and Two Adult Godfather tickets the arrays will show [Avengers, Godfather] [2, 2] [1, 0]  
  },
  created(){
    this.$root.$refs.App = this; // This is to keep connectivity with the other components, basically sending data back and forth
  },
  methods:{
    apiCall(call){
      axios
      .get(call)
      .then((response) => {
        this.apiData = response.data.results.slice(0,8);
        console.log(this.tvshows);
        console.log(call)
      }) // this is the API call of course, I used an attribute to send into it so I can change the api on the fly
    },
    getTotal()
    {
      let temp = 0;
      for (let i = 0; i < this.movAdAmount.length; i++)
      {
        console.log(this.movAdAmount)
        temp += (this.movAdAmount[i] * 9.99)
      }
      for (let i=0 ; i <this.movChAmount.length; i++)
      {
        temp+= (this.movChAmount[i] * 4.99)
      }
      console.log(temp);
      this.totals = temp;
    }, // this sets the total, which is the subtotals with the tax added on, this runs every time a ticket is added or removed
    cardClickAd(item){
      if (this.chosenMov.includes(item) == false)
      {
        this.chosenMov.push(item);
        this.movAdAmount.push(1);
        this.movChAmount.push(0);
      } // this activates when a card's adult ticket is clicked, if that movie isnt in the array then it adds it and if it IS in the array it simply adds one to the ticket amount array at the specific location 
      else{
        this.movAdAmount[this.chosenMov.indexOf(item)] += 1
        this.$root.$refs.CartComp.updateTick()
      }
      console.log(this.chosenMov);
      console.log(this.movAdAmount);
      console.log(this.movChAmount); // this was just to debug
      this.getTotal()
    },
    cardClickCh(item){
      if (this.chosenMov.includes(item) == false)
      {
        this.chosenMov.push(item);
        this.movChAmount.push(1);
        this.movAdAmount.push(0);
      } // this is the same as the previous method but for child tickets 
      else{
        this.movChAmount[this.chosenMov.indexOf(item)] += 1
        this.$root.$refs.CartComp.updateTick()
      }
      console.log(this.chosenMov);
      console.log(this.movAdAmount);
      console.log(this.movChAmount);
      this.getTotal()
    }
  },
  mounted(){
    this.apiCall("https://api.themoviedb.org/3/" + this.movieShows[this.$root.$refs.NavBar.media] + "?api_key=70ef7c62eee1244489c96681175a2a0f&language=en-US&page=1");
    // The API call activates as soon as the page loads
  }, 
}


</script>

<style>

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;

  color: #2c3e50;
  margin-top: 0px;
}
.button{
  background-color: transparent;
  border: 0;
  color: #fefbef;
  font-family: 'Open Sans', sans-serif;
  font-weight: 600;
  line-height: 1;
  margin-top: 1.5rem;
  width: 100%;
}
.footer{
  margin-top: 25px; 
  margin-bottom: 25px; 
  border-style:double; 
  border-width: 1px;
  padding: 5px;
  width: 90%;
  margin-left: auto;
  margin-right: auto;
}
</style>
