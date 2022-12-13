<template>
    <div class="row">
        <div class="col-sm">
        {{col1}}
        </div>
        <div class="col-sm">
        {{col2}}
        </div>
        <div class="col-sm">
        {{col3}}
        </div>
        <div class="col-sm">
        {{col4}}
        </div>
        <div class="col-sm">
        {{col5}}
        </div>
    </div>
</template>

<script>
// This is just a simple Row component that is different from the cart, since this I can just easily send data into to make up the header and footer of the chart showing the tickets

export default{
    name:"RowComp",
    data()
    {
        return{

        }
    },
    props:{
        col1: String,
        col2: String,
        col3: String,
        col4: String,
        col5: String
    }
}</script>