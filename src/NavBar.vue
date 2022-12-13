<template>
  <nav class="navbar navbar-expand-lg bg-light">
    <div class="container-fluid">
      <div class="container">
        <select name="choices" @change="onChange($event)" id="choices">
          <option value="0">Movies Now Playing</option>
          <option value="1">Popular Movies</option>
          <option value="2" selected>Upcoming Movies</option>
          <option value="3">Top Rated Movies</option>
          <!-- This is what helps change the API, see the method below -->
        </select>
      </div>
      <div class="centerx">
        <button >Cart: {{movTotal}}</button>
        <!-- this was going to be a button that takes you to a seperate page for your cart but I couldn't figure that out, so now the cart is the bottom of the page -->
      </div>
    </div>
  </nav>
</template>

<script>
export default {
  name: "NavBar",
  components:{
  },
  data(){
      return{
        movTotal: 0,
        media: 2
      } 
      // movTotal is what keeps track of how many tickets total there are, and media is what tracks what the API should be returning (the choice box thing)
  },
  created(){
    this.$root.$refs.NavBar = this; // This is to keep connectivity with the other components, basically sending data back and forth
  },
  methods: {
    onChange(event){
      console.log(event.target.value)
      this.media = event.target.value;
      this.$root.$refs.App.apiCall("https://api.themoviedb.org/3/" + this.$root.$refs.App.tvshows[this.media] + "?api_key=70ef7c62eee1244489c96681175a2a0f&language=en-US&page=1");
      // This is what changes the API
    },
    cartClick()
    {
      this.movTotal++;
    },
    cartRem()
    {
      this.movTotal--;
    }
// these two just add and remove from the movTotal
  },
  mounted(){
    // nothing to run on load sadly 
  }
};


</script>
