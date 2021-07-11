<template>
  <nav class="navbar navbar-dark bg-dark">
    <span class="navbar-brand mr-2 mb-0 h1">
      <img
        src="/logo.svg"
        width="30"
        height="30"
        class="d-inline-block align-top"
        alt=""
      />
      Localbitcoins Selling Dashboard</span
    >
    <h5>
      <span class="badge badge-warning mt-2 mx-3">BTC $ {{ btc_price }}</span>
      <span class="badge badge-success mt-2"
        >DolarToday Bs. {{ dolartoday | toDecimalFormat }}</span
      >
    </h5>
  </nav>
</template>
<script>
import axios from "axios";
const CORS_SERVER = process.env.VUE_APP_CORS_SERVER;

export default {
  data() {
    return {
      dolartoday: null,
      btc_price: null
    };
  },
  mounted() {
    let url = CORS_SERVER + "https://www.bitven.com/assets/js/rates.js";
    axios
      .get(url)
      .then(resp => {
        this.dolartoday = parseFloat(resp.data.USD_TO_BSF_RATE).toFixed(2);
        this.btc_price = parseFloat(resp.data.BTC_TO_USD_RATE).toFixed(2);
      });
  },
  filters: {
    toDecimalFormat: function(value) {
      if (isNaN(parseFloat(value))) {
        return value;
      }
      var formatter = new Intl.NumberFormat("es-VE", {
        style: "decimal",
        minimumFractionDigits: 0
      });
      return formatter.format(value);
    }
  }
};
</script>
