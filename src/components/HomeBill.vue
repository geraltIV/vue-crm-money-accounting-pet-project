<template>
  <div class="col s12 m6 l4">
    <div class="card light-blue bill-card">
      <div class="card-content white-text">
        <span class="card-title">Счет в валюте</span>

        <p
          v-for="currency in currencies"
          :key="currency"
          class="currency-line"
        >
          <span>{{ getCurrency(currency) | currency(currency) }}</span>
        </p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "HomeBill",
  data() {
    return {
      currencies: ["UAH", "USD", "EUR"],
    };
  },
  props: {
    rates: {
      type: Object,
      default() {
        return {};
      },
    },
  },
  computed: {
    base() {
      return (
        this.$store.getters.info.bill / (this.rates["UAH"] / this.rates["EUR"])
      );
    },
  },
  methods: {
    getCurrency(currency) {
      return Math.floor(this.base * this.rates[currency]);
    },
  },
};
</script>

<style>
</style>