<template>
<div class="grid grid-cols-9" v-if="coinsfiltradas.length <= 0 ">
    <div class="col-span-9 w-full h-full flex flex-row items-center gap-2 justify-center">
        <div class="c-loader mt-56"></div>
        <h2 class="text-center mt-56">Carregando...</h2>
    </div>
</div>

<div class="grid lg:grid-cols-9 grid-rows-1 sm:grid-cols-5" v-else>
      <div class="lg:col-span-1 row-span-1 sm:row-span-2"></div>
      <div class="col-span-2 flex flex-row" v-for="coin in coinsfiltradas" v-bind:key="coin.name" >
        <Card_Descript  :nome='coin.name' :id="coin.id" :total_volume="coin.total_volume" :symbol="coin.symbol" :img="coin.image" :price="coin.current_price" :variation="coin.price_change_percentage_24h" :market_cap="coin.market_cap"/>
      </div>
      <Menu />
      <bar_top/>
</div>
</template>


  
<script>
import Menu from './components/sidebar-m.vue'
import Card_Descript from './components/Card-Des.vue'
import bar_top from './components/bar-top.vue'


export default {
    name: 'App',
    data(){
    return {
      coins:[],
      coinsfiltradas:[],
      
    }
  },
  components: {
    Menu,
    Card_Descript,
    bar_top,
  },

  async mounted(){
     setInterval(async ()=>{
       const req = await fetch('https://api.coingecko.com/api/v3/coins/markets?vs_currency=usd&order=market_cap_desc&per_page=100&page=1&sparkline=false')
       const data = await req.json()
       this.coins = data
      for(var i=0; i<this.coins.length; i++){
       var coins_id = this.coins[i].symbol;
      if(coins_id == 'eth' || coins_id == 'luna' || coins_id =='atom' || coins_id =='btc' || coins_id == 'dacxi'){
        this.coinsfiltradas.push(this.coins[i])
       }
      }
      if(this.coinsfiltradas.length > 4){
        for(var a=0; a<4; a++){
          this.coinsfiltradas.shift()
        }
      }

    
    }, 60000)
    
  },
   
}
</script>
<style>

body{
  width: 100vw;
  height: 100vh;
  background-color: rgb(245, 245, 245);
  
}
.c-loader {
  animation: is-rotating 1s infinite;
  border: 6px solid #e5e5e5;
  border-radius: 50%;
  border-top-color: #51d4db;
  height: 50px;
  width: 50px;
}

@keyframes is-rotating {
  to {
    transform: rotate(1turn);
  }
}

</style>
