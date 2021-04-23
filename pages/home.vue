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
        .get("https://api.gopax.co.kr/trading-pairs/XRP-KRW/ticker", {
          crossDomain: true,
        })
        .then(function() {
          this.res = res.data;
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
