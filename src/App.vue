<template>

    <div class="divDepo center">
    <div style="margin-top: 1rem;" class="depoList">
        <div class="depoElemOne">
        <h3 class="AppNameh3">Invest Calc</h3>
        <h4>Ваше Депо</h4>  
        <input class="depo" type="text" ref="depoValue" :style="{borderColor: compliteWrite > 0 ? 'lime' : 'black' }"  /> <strong>$</strong> 
        <button @click="depoMoney10(), depoMoney5(), compliteWrite = 1" class="depoBtn">Подтвердить</button>
        </div>
    </div>


    <div class="priceHigh">
        <h4 style="margin-top: 0.5rem;"> Посмотрите максимальную стоймость актива за последний год </h4>
        <h4 style="margin-top: -1rem;">На сколько процентов сейчас цена ниже от годового максимума</h4>        
        <input type="text" ref="priceValue" :style="{borderColor: compliteWrite > 1 ? 'lime' : 'black' }"/> <strong>%</strong>    
        <button @click="yearPrice(), compliteWrite = 2">Подтвердить</button>
        <h3 v-if="priceCount === 1" >{{valueprice[priceCount].text}}</h3>
    </div>

    <StonksItem :depomin5="this.depo5" :depomin10="this.depo10" :pricecount="this.priceCount" :writecomp="this.compliteWrite"></StonksItem>

    </div>

</template>

<script>

import StonksItem from './components/StonksItem'

export default{

  data(){
    return{
  priceCount: 0,
  depo10: 0,
  depo5: 0,
  compliteWrite: 0,
  valueprice: [
    {text: ""},
    {text: "Закуп не производится. Ждите большего падения цены"},  
]
}
},

methods:{
    yearPrice(){
        if(this.$refs.priceValue.value < 20){
        this.priceCount = 1
        }
        else if (this.$refs.priceValue.value > 19 && this.$refs.priceValue.value < 30 ){
            this.priceCount = 2
        }
        else if (this.$refs.priceValue.value > 29 && this.$refs.priceValue.value < 40 ){
            this.priceCount = 3
        }
        else if (this.$refs.priceValue.value > 39){
            this.priceCount = 4
        }
        },

    depoMoney10(){
        this.depo10 = ((this.$refs.depoValue.value / 100) * 10)
    },
    depoMoney5(){
        this.depo5 = ((this.$refs.depoValue.value / 100) * 5)
    },

},


components:{ StonksItem },


}
</script>

<style>

.center{
text-align: center;
justify-content: center;
}

.depoList{
border: solid;
border-radius: 15px;
background: silver;
margin-left: 1rem;
margin-right: 1rem;
font-family: cursive;
}

.depoList h4{
margin-bottom: 1rem;
margin-top: 0rem;
}

.AppNameh3{
border: solid;
border-radius: 13px;
margin-left: 0.5rem;
max-width: 15%;
margin-bottom: -1rem;
margin-top: 0.5rem;
background: royalblue;
color: white;
border-color: black;
font-family: cursive;
padding-bottom: 0.4rem;
padding-top: 0.2rem;
}

.divDepo{
    border: solid;
    background: ghostwhite;
    border-radius: 20px;
}

.depoElemOne button{
margin-left: 1.2rem;
max-width: 50%;
border: solid;
border-radius: 15px;
background: royalblue;
border-color: royalblue;
color: white;
font-family: cursive;
padding-bottom: 0.2rem;
}

.depoElemOne input{
justify-self: center;
max-width: 20%;
border: solid;
border-radius: 10px;
}

.priceHigh{
border: solid;
border-radius: 15px;
margin-top: 1rem;
background: silver;
margin-bottom: 1rem;
margin-left: 1rem;
margin-right: 1rem;
font-family: cursive;
}

.priceHigh input{
max-width: 20%;
margin-bottom: 1rem;
border: solid;
border-radius: 10px;
}

.priceHigh button{
margin-left: 0.65rem;
max-width: 50%;
border: solid;
border-radius: 15px;
background: royalblue;
border-color: royalblue;
color: white;
font-family: cursive;
padding-bottom: 0.2rem;
}

.priceHigh h3{
margin: 1rem;
}

.depo{
margin-bottom: 1rem;
}

</style>
