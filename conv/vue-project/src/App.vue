<template>
  <div id="app">
    <div class="converter">
      <h3>Currency Converter</h3>
      <p>Enter Amount: 
        <input v-model="sum">
      </p> 
      <p>Convert From: 
        <select v-model="from">
          <option value="USD">USD</option>
          <option value="EUR">EUR</option>
          <option value="GBP">GBP</option>
          <option value="AUD">AUD</option>
          <option value="ARS">ARS</option>
          <option value="BMD">BMD</option>
          <option value="BRL">BRL</option>
          <option value="XAF">XAF</option>
          <option value="ETB">ETB</option>
          <option value="KYD">KYD</option>
          <option value="KRW">KRW</option>
        </select>
      </p>
      <p>Convert To: 
        <select v-model="to">
          <option value="UAH">UAH</option>
        </select>
      </p>
      <button @click="convert()">Конвертувати</button>
      <p>{{result}}</p>
     </div>
</div>
</template>

<script>
import axios from 'axios';
import cc from 'currency-codes';

export default {
  data() {
      return {
          bank_conv: [],
          sum:'',
          from:'',
          to:'',
          rateCross: 0,
          result: ''
      }
   },	 
   mounted: function(){
   },
   methods: {
      convert() {
        axios.get("https://api.monobank.ua/bank/currency").then((response) => {
          this.bank_conv = response.data;
          let findObject = this.bank_conv.find(item => {
          console.log("1", item.currencyCodeA);
          console.log("2",this.from);
          return item.currencyCodeA == cc.code(this.from).number && item.currencyCodeB == cc.code(this.to).number;
        })
        console.log(findObject);
        this.result = this.sum * findObject.rateBuy;

        }) 
        
           
      }
   },
};
</script>

<style scoped>

</style>