<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <div class="container">
      <!-- выбор валюты 1-->
      <div class="currency-select">
        <select name="first_currency" id="first_currency" @change="convert()" v-model="currencyOne">
          <!-- валюта -->
          <option value="EUR">EUR</option>
          <option value="GBP">GBP</option>
          <option value="RUB">RUB</option>
          <option value="USD">USD</option>
        </select>
        <input type="number" name="input-one" id="input-one" v-model="amountOne" @input="convert()">
      </div>
      <div>
        <button @click="convert()">Конвертировать</button>
        <p id="baseValue">1 {{ currencyOne }} = {{ rate }} {{ currencyTwo }}</p>
      </div>

       <!-- выбор валюты 2-->
       <div class="currency-select">
        <select name="second_currency" id="second_currency" v-model="currencyTwo">
          <!-- валюта -->
          <option value="EUR">EUR</option>
          <option value="GBP">GBP</option>
          <option value="RUB">RUB</option>
          <option value="USD">USD</option>
        </select>
        <input type="number" name="input-two" disabled v-model="amountTwo">
      </div>

      <div>
        <p>Последнее обновление курса: {{ update }}</p>
      </div>

    </div>
  </div>
</template>

<script>
export default {
data() {
  return {
    data: [],
    currencyOne: 'RUB',
    currencyTwo: 'EUR',
    rate: '',
    update: '',
    amountOne: 1,
    amountTwo: 0,
  }
},
methods: {
  convert() {
    fetch(`https://v6.exchangerate-api.com/v6/8d45a3826755fceac6db2437/latest/${this.currencyOne}`)
    .then(res => res.json())
    .then(data => {
    this.data = data
    this.rate = data.conversion_rates[this.currencyTwo]
    this.amountTwo = this.amountOne * this.rate.toFixed(2)
    const newDate = data.time_last_update_utc.substring(0,17)
    this.update = newDate
    });
  },
  calculate() {
    const tempValue = this.currencyOne
    this.currencyOne = this.currencyTwo
    this.currencyTwo = tempValue
    this.convert()
  }
},
mounted() {
  this.convert();
}
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

</style>
