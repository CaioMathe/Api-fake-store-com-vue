<template>
<div class=" w-12/12 h-screen">
    
    <div class=" relative text-center w-full bg-green-500 shadow-lg rounded-xl h-2/6  mt-20">   
        <div class="flex flex-col items-center">
            <h1 class="m-4 text-white">{{nome}} - <span class="uppercase muted text-opacity-40 text-green-100">{{symbol}}</span> </h1>  
            <img :src="img" class="w-3/6 h-auto rounded-full">
            <h1 class="mt-4 text-white">{{price.toLocaleString("en-US", {style:"currency", currency:"USD"})}}</h1>
        </div>
    </div>

    <div class=" w-full  bg-white shadow-2xl rounded-xl h-3/5  -mt-20">
        <div class="flex flex-col items-center">
            <h1 class="  mt-20  font-bold">24h:</h1>
            <h2>{{variation.toFixed(2)}}%</h2>
            <h1 class="  mt-4  font-bold">Market Cap:</h1>
            <h2>{{market_cap.toLocaleString("en-US", {style:"currency", currency:"USD"})}}</h2>
             <h1 class="  mt-4  font-bold">Total Volume:</h1>
            <h2>{{total_volume.toLocaleString("en-US", {style:"currency", currency:"USD"})}}</h2>

            <h1 class="  mt-4  font-bold">Price on date:</h1>
            <div class="mb-2 mt-3 flex ">
                <input type="text" :id="symbol" class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5    dark:focus:ring-blue-500 dark:focus:border-blue-500" placeholder="Ex: 20-01-2020..." required>
            </div>
            <button v-on:click="search" class="bg-green-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded-full shadow-2xl">
            Search
            </button>
        </div>
    
    </div>
        <div :id="symbol+symbol" class=" invisible relative w-full bg-green-500 shadow-2xl rounded-xl h-1/5  -mt-36">
            <div class="flex flex-col items-center">
                <button v-on:click="close" class=" absolute  right-0 font-bold w-6 h-6 hover:bg-red-600 text-center rounded-tr-xl">X</button>
                <h2 class="mt-6 text-white font-semibold">The price in date:</h2>
                <h2 class="mt-2 text-white">{{result.toLocaleString("en-US", {style:"currency", currency:"USD"})}}</h2>
                
            </div>
        </div>

</div>


</template>

<script>


export default {
    props:{
        nome: String,
        id: String,
        symbol: String,
        img: Image,
        price: String,
        variation: String,
        market_cap: String,
        total_volume: String,
    },
    data() {
        return{
            result:[],
        }
    },
    methods:{
        search: async function (){
        document.getElementById(`${this.symbol+this.symbol}`).classList.remove('invisible')
        var date = document.getElementById(`${this.symbol}`).value 
        const req = await fetch(`https://api.coingecko.com/api/v3/coins/${this.id.toLowerCase()}/history?date=${date}`)
        const data = await req.json()
        const value_usd = data.market_data.current_price.usd
        this.result = value_usd.toLocaleString("en-US", {style:"currency", currency:"USD"});
        

        },
        close: function(){
            document.getElementById(`${this.symbol+this.symbol}`).classList.add('invisible')

        }
    }
}
</script>
<style>

h1{
    text-shadow: #CCC 1px 0 10px;
}

</style>