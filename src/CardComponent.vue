<template>
    <div class="card" style="width: 18rem;">
        <img :src=imgPath class="card-img-top" alt="...">
        <div class="card-body">
            <h5 class="card-title">{{movie.title}}</h5>
            <p class="card-text">{{movie.overview}}</p>
            <a href="#" class="btn btn-primary" name="adults" ref="adult" id="adults" @click="cartAd($event)" style="margin-right: 3px;">Adult Ticket</a>
            <a href="#" class="btn btn-primary" name="children" ref="children" id="children" @click="cartChld($event)">Child Ticket</a>
        </div>
    </div>
</template>

<script>
    export default({
        name: 'CardComponent',
        props:{
            movie: Object
                },
            data(){
            return {
                imgPath:  "https://image.tmdb.org/t/p/w500/" + this.movie.poster_path,
                adultTick: 0, 
                childTick: 0
            }
        },
        created(){
            this.$root.$refs.CardComponent = this; // This is to keep connectivity with the other components, basically sending data back and forth
        },  
        methods:{
            cartAd(event)
            {
                this.adultTick++
                this.$root.$refs.NavBar.cartClick(this.movie);
                this.$root.$refs.App.cardClickAd(this.movie.title);
            },
            cartChld(event)
            {
                this.childTick++
                this.$root.$refs.NavBar.cartClick(this.movie);
                this.$root.$refs.App.cardClickCh(this.movie.title);
            }
            // these two methods activate when either adult or child ticket is clicked and then activates the other methods in the other components relating to the tickets
        }
        
    })

</script>

