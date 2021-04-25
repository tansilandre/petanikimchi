<template>
  <div class="px-3 py-2">
    <b><h3>Petani Kimchi naik HAJI</h3></b>
    <span>Remittance</span>
    <select
      v-model="selectedRemittance"
      class="form-select my-3"
      aria-label="Default select example"
      @change="getAllCoins(coins, selectedRemittance)"
    >
      <option v-for="item in remittance" :key="item.name" :value="item.value">{{
        `${item.name} ${item.value}%`
      }}</option>
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
        <tr v-for="item in tableData" :key="item.coinUpper">
          <th scope="row">1</th>
          <td>{{ item.coinUpper }}</td>
          <td>{{ item.kimchi ? item.kimchi.toFixed(2) : "-" }}</td>
          <td>{{ item.prices.indodax ? "Rp" +  formatPrice(item.prices.indodax)  : "-" }}</td>
          <td>{{ item.prices.gopax ? formatPrice(item.prices.gopax)+ " 원" : "-" }}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>

import axios from "axios";
export default {
  created() {
    this.getAllCoins(this.coins,this.remittance)
  },
  methods: {
    formatPrice(price){
      let val = (price/1).toFixed(2).replace(".",",")
      return val.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ".")
    },
    getAllCoins(coins,remittance){
      this.tableData = []
      remittance? remittance : remittance == 12.75
      coins.forEach(item => {
        this.getTableData(item.name,remittance,item.fee)
      });
    },
    getTableData(coin,remittance,fee){
      let that = this
      remittance ? remittance : remittance == 12.75
      let url = `http://185.201.8.243:3000/api/kimchi?coin=${coin}&remittance=${remittance}&fee=${fee}`
      axios.get(url)
      .then(function (res) {
        that.tableData.push(res.data)
        console.log("this.tableData")
        // console.log(this.tableData)
      })
      .catch(function (err) {
        console.log("ERROR BOSQ" + err)
      })
    }
  },
  computed: {},
  data() {
    return {
      selectedRemittance: 12.75,
      coins: [
        { name: "XRP", fee: 1 },
        { name: "XLM", fee: 0.02 },
        { name: "BCH", fee: 0.0005 },
        { name: "LTC", fee: 0.01 },
        { name: "EOS", fee: 0.1 }
      ],
      remittance: [
        {name: "cukong", value: 12.75},
        {name: "sentbe", value: 12.95},
        {name: "hanpass", value: 12.8},
      ],
      tableData: []
    };
  }
};
</script>
