<template>
  <div>
    <CreditCard
      :showInputs="showInputs"
      :padding="padding"
      :cardNumber.sync="cardNumber"
      :name.sync="name"
      :expirationDate.sync="expirationDate"
      :cvc.sync="cvc"
      :flip.sync="flip"
    />
    <div
      style="
        margin-top: 10px;
        backdrop-filter: brightness(120%);
        border-radius: 24px;
        padding: 4px;
      "
    >
      <h4>Basic Demo:</h4>
      <div style="display: flex">
        <label><input type="checkbox" v-model="showInputs" />Input</label>

        <label><input type="checkbox" v-model="flip" />Prog-Flip</label>

        <label
          ><input
            style="color: white"
            :disabled="!showInputs"
            min="0"
            type="number"
            v-model="padding"
          />Padding{{ showInputs ? '' : ' | Disabled' }}</label
        >
      </div>
      <div>
        <button @click="randomData">RANDOM</button>
      </div>
    </div>
  </div>
</template>

<script>
import CreditCard from './credit-card.vue';
export default {
  components: {
    CreditCard,
  },

  data: () => ({
    showInputs: true,
    padding: 12,
    cardNumber: 6290499495563199,
    name: 'John Doe',
    expirationDate: '2026-12',
    cvc: 123,
    flip: false,
  }),

  methods: {
    randomData() {
      this.showInputs = Math.random() < 0.5;
      this.padding = Math.random() * (100 - 0) + 0;
    },
  },
};
</script>

<style>
body {
  background: linear-gradient(to right, #4e54c8, #8f94fb);
  font-family: 'Century Gothic', CenturyGothic, Geneva, AppleGothic, sans-serif;
}
</style>
