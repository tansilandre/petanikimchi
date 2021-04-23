<template>
  <div class="px-3 py-2">
    <b><h3>Petani Kimchi naik HAJI</h3></b>
    <span>Remittance</span>
    <select
      v-model="selectedRemittance"
      class="form-select my-3"
      aria-label="Default select example"
    >
      <option value="1" selected>Cukong {{ remittance.cukong }} %</option>
      <option value="1">Sentbe {{ remittance.sentbe }} %</option>
      <option value="2">Hanpass {{ remittance.hanpass }} %</option>
    </select>

    <table class="mt-2 table small">
      <thead>
        <tr>
          <th scope="col">#</th>
          <th scope="col">Coin</th>
          <th scope="col">Kimchi</th>
          <th scope="col">Indodax</th>
          <th scope="col">Gopax</th>
        </tr>
      </thead>

      <tbody>
        <tr v-for="item in tableData" :key="item.id">
          <th scope="row">1</th>
          <td>{{ item.id }}</td>
          <td>{{ item.kimchi ? item.kimchi : "-" }}</td>
          <td>{{ item.indodax }}</td>
          <td>{{ item.gopax }}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import axios from "axios";
export default {
  created() {
    // this.coin.forEach(coin => {
    //   this.getCoinValue(coin);
    // });
    this.getCoinValue(this.coin[0]);
  },
  methods: {
    calculateKimchi(remittance, coin, indodaxPrice, gopaxPrice) {
      let modal = 100000000;
      let hasil;
      let kimchi;

      // 0. transfer money to indodax (6500 + 5000)
      hasil = 100000000 - 11500;

      // 1. buy from indodax (300rb)
      hasil = hasil - (0.3 / 100) * hasil;
      hasil = hasil / indodaxPrice;

      // 2. transfer to gopax
      hasil = -coin.fee;

      // 3. sell coin  in gopax
      hasil = hasil * gopaxPrice;
      hasil = hasil - (hasil * 0.2) / 100;

      // 4. withdraw  from gopax
      hasil - 1000 * remittance;
      kimchi = ((hasil - modal) / modal) * 100;
    },

    getCoinValue(coin) {
      let priceIndodax = 0;
      let priceGopax = 0;
      let that = this;

      let one = this.linkIndodax(coin.name);
      let two = this.linkGopax(coin.name);

      const requestOne = axios.get(one);
      const requestTwo = axios.get(two);

      axios
        .all([requestOne, requestTwo])
        .then(
          axios.spread((...responses) => {
            const responseOne = responses[0];
            const responseTwo = responses[1];
            priceIndodax = responseOne.data.ticker.sell;
            priceGopax = responseTwo.data.bid;
            that.tableData.push({
              id: coin.name,
              indodax: that.priceIndodax,
              gopax: that.priceGopax,
              kimchi: that.calculateKimchi(
                that.selectedRemittance,
                coin.fee,
                priceIndodax,
                priceGopax
              )
            });
          })
        )
        .catch(errors => {});
    },
    linkGopax(coin) {
      console.log(coin);
      let link =
        "https://api.gopax.co.kr/trading-pairs/" +
        coin.toUpperCase() +
        "-KRW/ticker";
      return link;
    },
    linkIndodax(coin) {
      let link = "https://indodax.com/api/ticker/" + coin.toLowerCase() + "idr";
      return link;
    }
  },
  computed: {},
  data() {
    return {
      selectedRemittance: 0,
      coin: [
        { name: "XRP", fee: 1 },
        { name: "XLM", fee: 0.02 },
        { name: "BCH", fee: 0.0005 },
        { name: "LTC", fee: 0.01 },
        { name: "EOS", fee: 0.1 }
      ],
      priceIndodax: 1,
      priceGopax: 0,
      remittance: {
        cukong: 12.75,
        sentbe: 12.95,
        hanpass: 12.8
      },
      tableData: []
    };
  }
};
</script>
