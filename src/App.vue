<template>
  <div id="app">
    <header>
      Last updated - {{ rateLastUpdated }}
    </header>
    <main>
      <div
        v-for="base in bases"
        class="currencyRow"
      >
        <div class="currencyEntry kroner">
          {{ base }} kr
        </div>
        <div class="currencyEntry dollar">
          ${{ convertToUSD(base) }}
        </div>
      </div>
    </main>
  </div>
</template>

<script>
export default {
  name: 'app',
  data () {
    return {
      rateLoaded: false,
      rateLoading: true,
      rateLastUpdated: '2017-08-25',
      rate: 0.15873,
      bases: [
        1,
        10,
        25,
        50,
        100,
        125,
        150,
        175,
        500,
        1000
      ]
    }
  },
  methods: {
    convertToUSD (kroner) {
      return Math.round(kroner * this.rate * 100) / 100
    },
    loadExchangeRate () {
      const conversionURL = `https://api.fixer.io/latest?base=DKK&symbols=USD`

      this.$http
        .get(conversionURL)
        .then((response) => {
          this.rateLoaded = true
          this.rateLoading = false

          const { body } = response
          this.rateLastUpdated = body.date
          this.rate = body.rates.USD
        })
        .catch((error) => {
          this.rateLoaded = false
          this.rateLoading = false
          console.log('error getting newest rate', error)
        })
    }
  },
  mounted () {
    this.loadExchangeRate()
  }
}
</script>

<style>
.currencyRow {
  display: flex;
  width: 100%;
  background-color: rgb(30, 162, 32);
}

.currencyRow:nth-child(even) {
  background-color: rgb(33, 168, 35);
}

.currencyEntry {
  flex: 1;
  height: 70px;
  line-height: 70px;
  text-align: center;
  color: #fff;
  font-size: 1.8em;
}

* , :after, :before {
  box-sizing: border-box;
}

html {
  height: 100%;
  color: #404040;
}

body {
  margin: 0;
  height: 100%;
}

#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  text-align: center;
  height: 100%;
}

header {
  margin: 0;
  height: 22px;
  line-height: 22px;
  font-size: 14px;
  text-align: center;
  background-color: rgb(46, 201, 49);
  color: #ffffff;
}
</style>
