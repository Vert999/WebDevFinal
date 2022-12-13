<template>
    <div class="row">
        <div class="col-sm">
        {{ movs }}
        </div>
        <div class="col-sm">
            {{ adultAmount }} * ${{adultPrice}}
        </div>
        <div class="col-sm">
            {{ childAmount }} * ${{childPrice}}
        </div>
        <div class="col-sm">
            ${{subtotal}}
        </div>
        <div class="col-sm">
            <div class="inline-block" style="display: inline-block;">
                <a href="#" class="btn btn-primary" @click="removeAd()" style="width: 45%; font-size: 70%; text-align: center; margin-right: 3px;">Remove Adult</a>
                <a href="#" class="btn btn-primary" @click="removeCh()" style="width: 45%; font-size: 70%; text-align: center;">Remove Child</a>
            </div>
        </div>
    </div>
</template>
<!-- I tried making the template look at clean as possible, but basically it just shows the movie title, the adult and child ticket amounts, the subtotal (for that ticket set) and two remove buttons for adult and child tickets alike -->
<script>
    export default{
        name: 'CartComp',
        data(){
            return{
                adultPrice: 9.99,
                childPrice: 4.99,
                adultAmount: this.$root.$refs.App.movAdAmount[this.$root.$refs.App.chosenMov.indexOf(this.movs)],
                childAmount: this.$root.$refs.App.movChAmount[this.$root.$refs.App.chosenMov.indexOf(this.movs)],
                subtotal: 0,
            }
        },
        props: {
            movs: String
            // only prop thats needed is the movie information, probably could have used props to get ticket information but it didnt come to mind
        }, 
        methods:{
            conLog()
            {
                console.log(this.adultPrice);
                console.log(this.adultAmount);
                console.log(this.subtotal);
                // I used this to debug, like a LOT, at one point I had like 100 errors so that was fun 
            },
            addAdult()
            {
                this.adultAmount ++;
            }, // these two just add to the adult or child amount to keep track of how many tickets there are, usually called from outside this component
            addChild(){
                this.childAmount ++;
            },
            setPrice()
            {
                this.subtotal = (this.adultPrice * this.adultAmount) + (this.childPrice * this.childAmount);
                this.total = this.subtotal + (this.subtotal * this.tax)
                this.subtotal = this.subtotal.toFixed(2)
            }, // this sets/updates the total and subtotal
            updateTick()
            {
                this.adultAmount = this.$root.$refs.App.movAdAmount[this.$root.$refs.App.chosenMov.indexOf(this.movs)];
                this.childAmount = this.$root.$refs.App.movChAmount[this.$root.$refs.App.chosenMov.indexOf(this.movs)];
                this.setPrice();
                this.$root.$refs.App.getTotal()
            }, // this just updates everything showing and also calls setPrice which updates the total and subtotal
            removeAd()
            {
                if (this.$root.$refs.App.movAdAmount[this.$root.$refs.App.chosenMov.indexOf(this.movs)] > 0) 
                {
                    this.$root.$refs.App.movAdAmount[this.$root.$refs.App.chosenMov.indexOf(this.movs)]-- ;
                    this.updateTick();
                    this.removedMov();
                }
            }, // these two remove functions remove 1 from the respective arrays when the remove button is clicked, unless its reached 0 in which case it does nothing 
            removeCh()
            {
                if (this.$root.$refs.App.movChAmount[this.$root.$refs.App.chosenMov.indexOf(this.movs)] > 0)
                {
                    this.$root.$refs.App.movChAmount[this.$root.$refs.App.chosenMov.indexOf(this.movs)]-- ;
                    this.updateTick();
                    this.removedMov();
                }
            },
            removedMov()
            {
                if (this.adultAmount ==0 && this.childAmount == 0)
                {
                    this.$root.$refs.App.movAdAmount.splice(this.$root.$refs.App.chosenMov.indexOf(this.movs), 1)
                    this.$root.$refs.App.chosenMov.splice(this.$root.$refs.App.chosenMov.indexOf(this.movs), 1)
                }
                // if there is no longer tickets for this movie, it will remove it from the respective arrays
            }

        },
        created(){
            this.$root.$refs.CartComp = this; // This is to keep connectivity with the other components, basically sending data back and forth
        },
        mounted() {
        console.log(this.movs);
        this.setPrice() // each time one of these row cartcomps are made, set price runs so it shows up immedietly and updates the total
        }

    }
</script>