<template>
  <div class="currency">
    <div class="container">
      <div class="row">
        <div class="col-lg-10 ml-auto mr-auto">
          <div class="row">
            <div class="col-lg-3">
              <label>From</label>
              <input type="text" v-model.trim="currency_from" class="form-control" placeholder="From..." @blur="handleCurrencyFrom"/>
              <small v-if="message.currency_from" class="form-text text-muted-err">Enter currency.</small>
              <small v-else-if="message.currency_from_err" class="form-text text-muted-err">Currency code not correct.</small>
            </div>
            <div class="col-lg-1">
              <i class="fa fa-exchange" aria-hidden="true" @click="exchangeCurrency"></i>
            </div>
            <div class="col-lg-3">
              <label>To</label>
              <input type="text" v-model.trim="currency_to" class="form-control" placeholder="To..." @blur="handleCurrencyTo"/>
              <small v-if="message.currency_to" class="form-text text-muted-err">Enter currency.</small>
              <small v-else-if="message.currency_to_err" class="form-text text-muted-err">Currency code not correct.</small>
            </div>
            <div class="col-lg-3">
              <label>Amount</label>
              <input type="number" v-model="amount" class="form-control" @blur="handleAmount"/>
              <small v-if="message.amount" class="form-text text-muted-err">Enter amount.</small>
            </div>
            <div class="col-lg-2">
              <button type="button" class="btn btn-primary" @click="convert_currency">
                <i class="fa fa-paper-plane" aria-hidden="true"></i>
              </button>
            </div>
          </div>
          <div v-if="result" class="row mt-4">
            <div class="col-lg-6 ml-auto mr-auto">
              <p>{{result}}</p>
              <div>You can spend these in the following countries:
              <ul v-for="(country, index) in countries" :key="index" class="list-group list-group-flush">
                <li class="list-group-item">
                  <span>{{country.name}}</span> &nbsp;
                  <img :src="country.flag" alt="" width="50px" height="30px">
                </li>
              </ul>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "Currency",
  props: {
    msg: String
  },
  data() {
    return {
      currency_from: "",
      currency_to: "",
      amount: 1,
      result: "",
      countries: [],
      rates: {},
      keyRates:[],
      count: 0,
      message: {
        currency_from: false,
        currency_from_err: false,
        currency_to: false,
        currency_to_err: false,
        amount: false,
      }
    };
  },

  created() {
    this.getRate()
  },

  methods: {
    async convert_currency() {
      this.handleCurrencyFrom()
      this.handleCurrencyTo()
      this.handleAmount()

      if (this.message.currency_from || this.message.currency_to || this.message.amount) {
        return false
      }

      const fromCurrency = this.currency_from.toUpperCase();
      const toCurrency = this.currency_to.toUpperCase();

      const exchangeRate = await this.getExchangeRate(fromCurrency, toCurrency);
      const countries = await this.getCountries(toCurrency);
      this.countries = countries;

      const convertedAmount = (this.amount * exchangeRate).toFixed(4);

      this.result = `${this.amount} ${fromCurrency} is worth ${convertedAmount} ${toCurrency}`;
    },

    getExchangeRate(fromCurrency, toCurrency) {
      try {
        const euro = 1 / this.rates[fromCurrency]; // convert fromCurrency to euro
        const exchangeRate = euro * this.rates[toCurrency]; // convert euro to toCurrency
        return exchangeRate;
      } catch (err) {
        throw new Error("Error:", err);
      }
    },

    async getRate() {
      const res = await axios.get("http://data.fixer.io/api/latest?access_key=1231af730ec3a786ece20a1501a3253e&format=1");
      this.rates = res.data.rates;
      this.keyRates = Object.keys(this.rates)
    },

    async getCountries(currencyCode) {
      try {
        const res = await axios.get(`https://restcountries.eu/rest/v2/currency/${currencyCode}`);
        return res.data;
      } catch (err) {
        throw new Error("Error:", err);
      }
    },

    handleCurrencyFrom() {
      this.message.currency_from = !this.currency_from

      this.message.currency_from_err = !this.keyRates.includes(this.currency_from.toUpperCase())
    },

    handleCurrencyTo() {
      this.message.currency_to = !this.currency_to
      this.message.currency_to_err = !this.keyRates.includes(this.currency_to.toUpperCase())
    },

    handleAmount() {
      this.message.amount = !this.amount
    },

    exchangeCurrency() {
      if (this.currency_from && this.currency_to) {
        [this.currency_from, this.currency_to] = [this.currency_to, this.currency_from];
      }
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
.currency {
  .fa-exchange {
    margin-top: 45px;
    cursor: pointer;
    font-size: 30px;
  }
  .btn-primary {
    margin-top: 35px;
    padding: 12px 15px;
  }
  .form-control {
    padding: 12px 15px;
  }
  .col-lg-3 {
    text-align: left;
  }
  label {
    font-weight: 700;
    font-size: 18px;
  }
  .text-muted-err {
    color: red;
  }
}
</style>
