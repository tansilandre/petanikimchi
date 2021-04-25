<template>
  <div>
    {{ res }}
  </div>
</template>

<script>
import axios from "axios";
export default {
  created() {
    this.getData();
  },
  methods: {
    async getData() {
      // this.res = await $http.$get('https://api.gopax.co.kr/trading-pairs/XRP-KRW/ticker')

      axios
        .get("http://185.201.8.243:3000/api/kimchi?coin=xrp", {
        })
        .then(function() {
          this.res = res.data;
          console.log(this.res)
        })
        .catch(function(error) {});
    }
    // async getData() {
    // this.res = await this.$http.$get('https://api.gopax.co.kr/trading-pairs/XRP-KRW/ticker')
    // }
  },
  data() {
    return {
      res: ""
    };
  }
};
</script>

<style></style>
