<template>
  <div>
    <CreditCard :showInputs="true"/>
  </div>
</template>

<script>
import CreditCard from './credit-card.vue';
export default {
  components: {
    CreditCard,
  },
};
</script>

<style>
body {
  background: linear-gradient(to right, #4e54c8, #8f94fb);
  font-family: 'Century Gothic', CenturyGothic, Geneva, AppleGothic, sans-serif;
}
</style>
