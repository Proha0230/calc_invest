<template>
    <div v-if="pricecount > 1" style="margin-bottom: 1rem;" class="depoList">
    <div class="depoElemOne">
    <h4 style="margin-top: 1rem;">Текущая цена актива</h4>  
    <input class="depo" type="text" ref="stonksInput" :style="{borderColor: writeComp > 2 ? 'lime' : 'black' }"/> <strong>$</strong>
    <button style="margin-left: 1.2rem;" @click="clicked(), writeComp = 3" class="depoBtn">Подтвердить</button>
    <h4 class="FirstBuyh4" v-if="pricecount === 2 && stonksCount === 1" >Первый закуп на 5% от Депо ({{ depomin5 }}$) : {{stonksItemsBuy5}}шт</h4>
    <h4 class="FirstBuyh4" v-if="pricecount === 3 && stonksCount === 1" >Первый закуп на 10% от Депо ({{ depomin10 }}$) : {{stonksItemsBuy10}}шт</h4>
    <h4 class="FirstBuyh4" v-if="pricecount === 4 && stonksCount === 1" >Первый закуп на 15% от Депо ({{ depomin5 + depomin10 }}$) : {{stonksItemsBuy15}}шт</h4>
    </div>

<div v-if="stonksCount === 1"  class="stonksList">

    <h4>График Усреднений</h4>

    <div class="stonksElem">
    <h4>Цена: {{ stonksValue1 }}$</h4>
    <h4>Кол-во {{ stockItems_10 }} шт</h4>
    <h4 v-if="pricecount === 2" >Ваша средняя: {{ Median10Drop }}</h4>
    <h4 v-if="pricecount === 3" >Ваша средняя: {{ Median10Drop }}</h4>
    <h4 v-if="pricecount === 4" >Ваша средняя: {{ Median10Drop }}</h4>
    </div>

    <div class="stonksElem">
    <h4>Цена: {{ stonksValue2 }}$</h4>
    <h4>Кол-во {{ stockItems_20 }} шт</h4>
    <h4 v-if="pricecount === 2" >Ваша средняя: {{ Median20Drop }}</h4>
    <h4 v-if="pricecount === 3" >Ваша средняя: {{ Median20Drop }}</h4>
    <h4 v-if="pricecount === 4" >Ваша средняя: {{ Median20Drop }}</h4>
    </div>
    
    <div class="stonksElem">
    <h4>Цена: {{ stonksValue3 }}$</h4>
    <h4>Кол-во {{  stockItems_30 }} шт</h4>
    <h4 v-if="pricecount === 2" >Ваша средняя: {{ Median30Drop }}</h4>
    <h4 v-if="pricecount === 3" >Ваша средняя: {{ Median30Drop }}</h4>
    <h4 v-if="pricecount === 4" >Ваша средняя: {{ Median30Drop }}</h4>
    </div> 

    <div class="stonksElem">
    <h4>Цена: {{ stonksValue4 }}$</h4>
    <h4>Кол-во {{ stockItems_40 }} шт</h4>
    <h4 v-if="pricecount === 2" >Ваша средняя: {{ Median40Drop }}</h4>
    <h4 v-if="pricecount === 3" >Ваша средняя: {{ Median40Drop }}</h4>
    <h4 v-if="pricecount === 4" >Ваша средняя: {{ Median40Drop }}</h4>
    </div>
    
    <div class="stonksElem">
    <h4>Цена: {{ stonksValue5 }}$</h4>
    <h4>Кол-во {{ stockItems_50 }} шт</h4>
    <h4 v-if="pricecount === 2" >Ваша средняя: {{ Median50Drop }}</h4>
    <h4 v-if="pricecount === 3" >Ваша средняя: {{ Median50Drop }}</h4>
    <h4 v-if="pricecount === 4" >Ваша средняя: {{ Median50Drop }}</h4>
    </div> 
</div>

</div>

</template>

<script>

export default{

data(){
  return{
    writeComp: this.writecomp,
    stonksCount: 0,
    stonksItemsBuy5: 0,   // сколько кол-во Акций покупать первоначально при 5% депо
    stonksItemsBuy10: 0,  // сколько кол-во Акций покупать первоначально при 10% депо
    stonksMediumBuy15: 0, // сколько кол-во Акций покупать первоначально при 15% депо
    stonksValue1: 0,      // цена Акций при -10% падения
    stockPurchased_10: 0, // на такую сумму куплено при первом усреднении
    stockPurchased_20: 0, // на такую сумму куплено при первом усреднении
    stockPurchased_30: 0, // на такую сумму куплено при первом усреднении
    stockPurchased_40: 0, // на такую сумму куплено при первом усреднении
    stockPurchased_50: 0, // на такую сумму куплено при первом усреднении

    stockItems_10:0, // кол-во штук для усреднения на -10%
    stockItems_20:0, // кол-во штук для усреднения на -20%
    stockItems_30:0, // кол-во штук для усреднения на -30%
    stockItems_40:0, // кол-во штук для усреднения на -40%
    stockItems_50:0, // кол-во штук для усреднения на -50%

    stonksValue2: 0, // цена Акций при -20% падения
    stonksValue3: 0, // цена Акций при -30% падения
    stonksValue4: 0, // цена Акций при -40% падения
    stonksValue5: 0, // цена Акций при -50% падения

    stockPurchased5: 0,  // на какую сумму было куплено акций первоначально на 5% от депо
    FirstAnd1DropBye: 0, // сумма общая первоначального закупа и 1 усрденения
    FirstAnd2DropBye: 0, // сумма общая первоначального закупа и 2 усрденения
    FirstAnd3DropBye: 0, // сумма общая первоначального закупа и 3 усрденения
    FirstAnd4DropBye: 0, // сумма общая первоначального закупа и 4 усрденения
    FirstAnd5DropBye: 0, // сумма общая первоначального закупа и 5 усрденения

  }
},



methods:{
stonksChange(){
    if(this.$refs.stonksInput.value > 0){
        this.stonksCount = 1
    }
},

// кол-во штук для усреднения на -10%
stockItem_10(){
    this.stockItems_10 = (this.depomin5 / this.stonksValue1).toFixed(0) 
},
// кол-во штук для усреднения на -20%
stockItem_20(){
    this.stockItems_20 = (this.depomin10 / this.stonksValue2).toFixed(0) 
},
// кол-во штук для усреднения на -30%
stockItem_30(){
    this.stockItems_30 = ((this.depomin5 + this.depomin10) / this.stonksValue3).toFixed(0) 
},
// кол-во штук для усреднения на -40%
stockItem_40(){
    this.stockItems_40 = ((this.depomin10 * 2) / this.stonksValue4).toFixed(0) 
},
// кол-во штук для усреднения на -50%
stockItem_50(){
    this.stockItems_50 = (((this.depomin10 * 2 ) + this.depomin5) / this.stonksValue5).toFixed(0) 
},


// общее кол-во акций купленных при первоначальном закупе и 1 усреднении
stockFirstBuyAnd_10ProcentDropBuy(){
    if(this.pricecount === 2){
    this.FirstAnd1DropBye = (+this.stockItems_10 + +this.stonksItemsBuy5)
    } else if(this.pricecount === 3){
    this.FirstAnd1DropBye = (+this.stockItems_10 + +this.stonksItemsBuy10) 
    } else if(this.pricecount === 4){
    this.FirstAnd1DropBye = (+this.stockItems_10 + +this.stonksItemsBuy15) 
    }
},

// на такую сумму куплено при 1 усреднении
stock_10ProcentDropBuy(){
    this.stockPurchased_10 = (this.stonksValue1 * ((this.depomin5 / this.stonksValue1).toFixed(0)))
},

// на такую сумму куплено при первом закупе
stock5FromDepoBuy(){
    if(this.pricecount === 2){
    this.stockPurchased5 = ((+this.$refs.stonksInput.value * +this.stonksItemsBuy5).toFixed(0))
    } else if(this.pricecount === 3){
    this.stockPurchased5 = ((+this.$refs.stonksInput.value * +this.stonksItemsBuy10).toFixed(0))
    } else if(this.pricecount === 4){
    this.stockPurchased5 = ((+this.$refs.stonksInput.value * +this.stonksItemsBuy15).toFixed(0))
    }
},
    
                // общее кол-во акций купленных при первоначальном закупе и 1+2 усреднении
                stockFirstBuyAnd_20ProcentDropBuy(){
                        this.FirstAnd2DropBye = (+this.FirstAnd1DropBye + +this.stockItems_20)
                    },

                // на такую сумму куплено при 2 усреднении
                stock_20ProcentDropBuy(){
                        this.stockPurchased_20 = (this.stonksValue2 * ((this.depomin10 / this.stonksValue2).toFixed(0)))
                    },

// общее кол-во акций купленных при первоначальном закупе и 1+2+3 усреднении
stockFirstBuyAnd_30ProcentDropBuy(){
    this.FirstAnd3DropBye = (+this.FirstAnd2DropBye + +this.stockItems_30)
},

// на такую сумму куплено при 3 усреднении
stock_30ProcentDropBuy(){
    this.stockPurchased_30 = (this.stonksValue3 * (((this.depomin10 + this.depomin5) / this.stonksValue3).toFixed(0)))
},

                // общее кол-во акций купленных при первоначальном закупе и 1+2+3+4 усреднении
                stockFirstBuyAnd_40ProcentDropBuy(){
                    this.FirstAnd4DropBye = (+this.FirstAnd3DropBye + +this.stockItems_40)
                },

                // на такую сумму куплено при 4 усреднении
                stock_40ProcentDropBuy(){
                    this.stockPurchased_40 = (this.stonksValue4 * (((this.depomin10 * 2) / this.stonksValue4).toFixed(0)))
                },

// общее кол-во акций купленных при первоначальном закупе и 1+2+3+4+5 усреднении
stockFirstBuyAnd_50ProcentDropBuy(){
this.FirstAnd5DropBye = (+this.FirstAnd4DropBye + +this.stockItems_50)
},

// на такую сумму куплено при 4 усреднении
stock_50ProcentDropBuy(){
this.stockPurchased_50 = (this.stonksValue5 * ((((this.depomin10 * 2) + this.depomin5) / this.stonksValue5).toFixed(0)))
},


stonksMedium5(){
    this.stonksItemsBuy5 = (+this.depomin5 / +this.$refs.stonksInput.value).toFixed(0)
},
stonksMedium10(){
    this.stonksItemsBuy10 = (+this.depomin10 / +this.$refs.stonksInput.value).toFixed(0)
},
stonksMedium15(){
    this.stonksItemsBuy15 = ((+this.depomin10 + +this.depomin5)/ +this.$refs.stonksInput.value).toFixed(0)
},


stocksPriceDrop10Procent(){
    this.stonksValue1 = ((this.$refs.stonksInput.value / 10) * 9).toFixed(2)
},
stocksPriceDrop20Procent(){
    this.stonksValue2 = ((this.$refs.stonksInput.value / 10) * 8).toFixed(2)
},
stocksPriceDrop30Procent(){
    this.stonksValue3 = ((this.$refs.stonksInput.value / 10) * 7).toFixed(2)
},
stocksPriceDrop40Procent(){
    this.stonksValue4 = ((this.$refs.stonksInput.value / 10) * 6).toFixed(2)
},
stocksPriceDrop50Procent(){
    this.stonksValue5 = ((this.$refs.stonksInput.value / 10) * 5).toFixed(2)
},



clicked(){
    this.stonksMedium5()
    this.stonksMedium10()
    this.stonksMedium15()
    this.stonksChange() 
    this.stocksPriceDrop10Procent() 
    this.stocksPriceDrop20Procent() 
    this.stocksPriceDrop30Procent() 
    this.stocksPriceDrop40Procent() 
    this.stocksPriceDrop50Procent()
    this.stock5FromDepoBuy()
    this.stock_10ProcentDropBuy()
    this.stock_20ProcentDropBuy()
    this.stock_30ProcentDropBuy()
    this.stock_40ProcentDropBuy()
    this.stock_50ProcentDropBuy()
    this.stockItem_10()
    this.stockItem_20()
    this.stockItem_30()
    this.stockItem_40()
    this.stockItem_50()
    this.stockFirstBuyAnd_10ProcentDropBuy()
    this.stockFirstBuyAnd_20ProcentDropBuy()
    this.stockFirstBuyAnd_30ProcentDropBuy()
    this.stockFirstBuyAnd_40ProcentDropBuy()
    this.stockFirstBuyAnd_50ProcentDropBuy()
}

},

computed:{
    Median10Drop() {
        return ((+this.stockPurchased5 + +this.stockPurchased_10) / +this.FirstAnd1DropBye).toFixed(2)
    },
    Median20Drop() {
        return ((+this.stockPurchased5 + +this.stockPurchased_10 + +this.stockPurchased_20) / +this.FirstAnd2DropBye).toFixed(2)
    },
    Median30Drop(){
        return ((+this.stockPurchased5 + +this.stockPurchased_10 + +this.stockPurchased_20 + +this.stockPurchased_30) / +this.FirstAnd3DropBye).toFixed(2)
    },
    Median40Drop(){
        return ((+this.stockPurchased5 + +this.stockPurchased_10 + +this.stockPurchased_20 + +this.stockPurchased_30 + +this.stockPurchased_40) / +this.FirstAnd4DropBye).toFixed(2)
    },
    Median50Drop(){
        return ((+this.stockPurchased5 + +this.stockPurchased_10 + +this.stockPurchased_20 + +this.stockPurchased_30 + +this.stockPurchased_40 + +this.stockPurchased_50) / +this.FirstAnd5DropBye).toFixed(2)
    },


},

props:{
    depomin5: Number,
    depomin10: Number,
    pricecount: Number,
    writecomp: Number,
}

}

</script>

<style>

.stonksElem{
    display:flex;
    justify-content: center;
}

.stonksElem h4{
    margin: 0.5rem;
}

.stonksList{
    margin-top: 1rem;
}

.FirstBuyh4{
    margin-top: 0rem;
}


</style>