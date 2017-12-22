<template>
  <div class="background">
    <div id="app" class="">
      <p style="width: 80%; margin: auto; text-shadow: 4px 4px 0px #ff0000; font-size: 46px; text-align: 'center'; font-style: italic; font-weight: 700;">
        <!-- {{ quote }} -->
        <span v-html="quote" style="padding: 8px; background-color: rgba(133,173,217,0.72);"></span>
        <span style="padding: 8px; background-color: rgba(133,173,217,0.72);">{{ author }}</span>
      </p>
      <p style="margin-top: 24px;">
        <span style="background-color: #111; color: #FFF; font-weight: 500; padding: 2px 6px;">
          <small>
            <!-- <svg class="fill-white"><use xlink="/static/svg/BTC-alt.svg"></svg> -->
            <img src="/static/svg/BTC-alt.svg" class="fill-white" alt="BTC logo">{{ BTCPrice }}
          </small>
        </span>
        <span style="background-color: #FFF; color: #333; padding: 2px 6px;">
          <small>
            <img src="/static/svg/ETH-alt.svg" alt="ETH logo">{{ETHPrice}}
          </small>
        </span>
        <span style="background-color: #111; color: #FFF; padding: 2px 6px;">
          <small>
            <img src="/static/svg/LTC-alt.svg" alt="LTC logo">{{LTCPrice}}
          </small>
        </span>
        <span style="background-color: #FFF; color: #333; padding: 2px 6px;">
          <small>
            <img src="/static/svg/XRP-alt.svg" alt="XRP logo">{{XRPPrice}}
          </small>
        </span>
      </p>
    </div>
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld'

export default {
  name: 'app',
  data: () => ({
    quote: '',
    author: '',
    BTCPrice: 0,
    ETHPrice: 0,
    LTCPrice: 0,
    XRPPrice: 0
  }),
  methods: {
    convertCurrency: function (num) {
      if (num) {
        return num.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ',')
      } else {
        return 0
      }
    }
  },
  mounted: function () {
  	let self = this
    axios.get('https://talaikis.com/api/quotes/random/')
      .then(response => {
          console.log(response.data.quote)
          self.quote = response.data.quote
          self.author = ' –' + response.data.author
        }
      )

    // axios.get('http://quotesondesign.com/wp-json/posts?filter[orderby]=rand&filter[posts_per_page]=1')
    //   .then(response => {
    //     self.quote = response.data[0].content
    //     self.author = ' –' + response.data[0].title
    //       // console.log()
    //       // console.log()
    //       // self.quote = response.data.quote
    //       // self.author = ' –' + response.data.author
    //     }
    //   )



    axios.get('https://api.coinbase.com/v2/prices/BTC-USD/spot')
      .then(response => {
        self.BTCPrice = self.convertCurrency(response.data.data.amount)
      })

    axios.get(`https://api.coinbase.com/v2/prices/ETH-USD/spot`)
      .then(response => {
        self.ETHPrice = self.convertCurrency(response.data.data.amount)
      })

    axios.get(`https://api.coinbase.com/v2/prices/LTC-USD/spot`)
      .then(response => {
        self.LTCPrice = self.convertCurrency(response.data.data.amount)
      })

    axios.get(`https://min-api.cryptocompare.com/data/price?fsym=XRP&tsyms=USD`)
      .then(response => {
        self.XRPPrice = response.data.USD.toFixed(2)
        console.log(response)
      })
  }
}
</script>

<style>
html {
  height: 100%;
  align-items: center;
}
body {
  background-image: linear-gradient(to bottom, #111, rgba(0, 255, 0, 0) 100%, transparent 30%);
  margin: 0px;
  height: 100%;
  /* align-items: center; */

  display: flex;
  flex: 0 0 100%;
}

.background {
  background-image: linear-gradient(to bottom, #111, rgba(0, 255, 0, 0) 100%, transparent 30%);
  overflow: auto;
  position: relative;
  height: 100%;
  align-items: center;

  margin: 0px;
  height: 100%;
  display: flex;
  flex: 0 0 100%;
}

.background:before {
  content: "";
  background: url('https://source.unsplash.com/random') no-repeat center center fixed;
  position: fixed;
  left: -5%;
  right: -5%;
  top: -5%;
  z-index: -1;

  display: block;

  background-size: 4000px;
  width: 200%;
  height: 140%;

  -webkit-filter: blur(5px);
  -moz-filter: blur(5px);
  -o-filter: blur(5px);
  -ms-filter: blur(5px);
  filter: blur(5px);
}

.fill-white {
  fill: white !important;
}

#app {
  width: 100%;
  font-family: 'Helvetica-Neue', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #fff;
  padding-bottom: 0px;
}

img {
  fill: green !important;
  height: 10px;
  width: 10px;
  padding-right: 3px;
}
</style>
