<template>
  <div
    style="
      display: flex;
      width: min-content;
      box-shadow: rgba(100, 100, 111, 0.2) 0px 7px 29px 0px;
      border-radius: 24px;
    "
    :style="{
      padding: showInputs ? padding+'px' : 0,
      background: showInputs ? 'white' : 'transparent',
    }"
  >
    <div class="container" :class="flip ? 'flip' : ''">
      <div
        class="card"
        style="
          z-index: 1;
          border-radius: 24px;
          box-shadow: rgba(100, 100, 111, 0.2) 0px 7px 29px 0px;
        "
      >
        <div
          class="front"
          style="border-radius: 24px; width: 340px; height: 200px"
          :style="{
            background:
              'linear-gradient(to right,' + getCardAcardNumberents + ')',
          }"
        >
          <h3
            style="position: absolute; top: 0px; right: 16px; font-weight: 200"
          >
            {{ creditCardType }}
          </h3>
          <div style="position: absolute; left: 16px">
            <span style="font-size: 12px"> Card Number </span>
            <h4 style="margin-top: 0px">
              {{ getNumber }}
            </h4>
          </div>

          <div style="position: absolute; bottom: 0px; left: 16px">
            <span style="font-size: 12px"> Name </span>
            <h4 style="margin-top: 0px">
              {{ name }}
            </h4>
          </div>

          <div style="position: absolute; bottom: 0px; right: 16px">
            <span style="font-size: 12px"> Expiration </span>
            <h4 style="margin-top: 0px">
              {{ getDate }}
            </h4>
          </div>
        </div>

        <div
          class="back"
          style="border-radius: 24px; width: 340px; height: 200px"
          :style="{
            background:
              'linear-gradient(to left,' + getCardAcardNumberents + ')',
          }"
        >
          <h3
            style="position: absolute; top: 0px; right: 16px; font-weight: 200"
          >
            {{ creditCardType }}
          </h3>
          <div style="position: absolute; bottom: 0px; right: 16px">
            <span style="font-size: 12px"> CVC </span>
            <h4 style="margin-top: 0px">
              {{ cvc }}
            </h4>
          </div>
        </div>
      </div>
    </div>
    <div
      v-if="showInputs"
      style="
        margin-left: 10px;
        margin-right: 10px;
        display: flex;
        flex-direction: column;
        justify-content: space-evenly;
      "
    >
      <div>
        <h5 style="margin: 0px; margin-bottom: 6px; color: rgb(50, 50, 50)">
          Card Number
        </h5>
        <input
          v-model="cardNumber"
          type="number"
          id="cardNumber"
          placeholder="cardNumber"
          @input="$emit('update:cardNumber', cardNumber)"
        />
      </div>
      <div>
        <h5 style="margin: 0px; margin-bottom: 6px; color: rgb(50, 50, 50)">
          Name
        </h5>
        <input
          v-model="name"
          type="name"
          id="name"
          placeholder="Name"
          @input="$emit('update:name', name)"
        />
      </div>
      <div style="display: flex">
        <div>
          <h5 style="margin: 0px; margin-bottom: 6px; color: rgb(50, 50, 50)">
            Date
          </h5>
          <input
            v-model="expirationDate"
            type="month"
            id="date"
            placeholder="date"
            @input="$emit('update:expirationDate', expirationDate)"
          />
        </div>
        <div style="margin: 8px" />
        <div>
          <h5 style="margin: 0px; margin-bottom: 6px; color: rgb(50, 50, 50)">
            CVC
          </h5>
          <input
            style="min-width: 24px"
            @focus="$emit('update:flip', true)"
            @blur="$emit('update:flip', false)"
            v-model="cvc"
            type="number"
            max="999"
            min="000"
            id="cvc"
            placeholder="CVC"
            @input="$emit('update:cvc', cvc)"
          />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    cardNumber: {
      default: 0,
      type: Number,
    },
    name: {
      default: undefined,
      type: String,
    },
    expirationDate: {
      default: undefined,
      type: String,
    },
    cvc: {
      default: undefined,
      type: Number,
    },

    showInputs: {
      default: true,
      type: Boolean,
    },
    flip: {
      default: false,
      type: Boolean,
    },
    padding: {
      default: 12,
      type: Number,
    },
  },

  data: () => ({}),

  mounted: function () {
    //this.$nextTick(() => {
    //  this.cardNumber = this.cardNumber;
    //});
  },

  computed: {
    getDate() {
      var date = this.expirationDate;
      var dateFrags = date.split('-');
      date = dateFrags[1] + '/' + dateFrags[0].slice(-2);

      return date.includes('undefined') ? null : date;
    },
    getNumber() {
      var number = this.cardNumber;
      if (!number) {
        number = '';
      }
      while ((number && number.length < 16) || number == '') {
        number += 'X';
      }
      return number;
    },
    getCardAcardNumberents() {
      switch (this.creditCardType) {
        case 'MASTERCARD':
          return '#EB001B, #FF5F00, #F79E1B';
        case 'VISA':
          return '#00b4db, #0083b0';
        case 'DISCOVER':
          return '#3c3b3f, #605c3c';
        case 'AMEX':
          return '#EB001B, #FF5F00, #F79E1B';
        case 'CHINA UNION PAY':
          return '#E31133, #00427D , #007C85';
        default:
          return '#1D3349, #06121E 55%';
      }
    },
    creditCardType() {
      let amex = new RegExp('^3[47][0-9]{13}$');
      let visa = new RegExp('^4[0-9]{12}(?:[0-9]{3})?$');
      let cup1 = new RegExp('^62[0-9]{14}[0-9]*$');
      let cup2 = new RegExp('^81[0-9]{14}[0-9]*$');

      let mastercard = new RegExp('^5[1-5][0-9]{14}$');
      let mastercard2 = new RegExp('^2[2-7][0-9]{14}$');

      let disco1 = new RegExp('^6011[0-9]{12}[0-9]*$');
      let disco2 = new RegExp('^62[24568][0-9]{13}[0-9]*$');
      let disco3 = new RegExp('^6[45][0-9]{14}[0-9]*$');

      let diners = new RegExp('^3[0689][0-9]{12}[0-9]*$');
      let jcb = new RegExp('^35[0-9]{14}[0-9]*$');

      if (visa.test(this.cardNumber)) {
        return 'VISA';
      }
      if (amex.test(this.cardNumber)) {
        return 'AMEX';
      }
      if (
        mastercard.test(this.cardNumber) ||
        mastercard2.test(this.cardNumber)
      ) {
        return 'MASTERCARD';
      }
      if (
        disco1.test(this.cardNumber) ||
        disco2.test(this.cardNumber) ||
        disco3.test(this.cardNumber)
      ) {
        return 'DISCOVER';
      }
      if (diners.test(this.cardNumber)) {
        return 'DINERS';
      }
      if (jcb.test(this.cardNumber)) {
        return 'JCB';
      }
      if (cup1.test(this.cardNumber) || cup2.test(this.cardNumber)) {
        return 'CHINA UNION PAY';
      }
      return undefined;
    },
  },
};
</script>

<style>
:is(h1, h2, h3, h4, h5, h6) {
  color: white;
  font-weight: 200;
}

span {
  color: rgb(230, 230, 230);
}

label {
  color: white;
}

input {
  background: transparent;
  backdrop-filter: brightness(97%);
  border: 1.3px solid rgb(190, 190, 190);
  border-radius: 10px;
  padding: 5px;
  color: rgb(50, 50, 50);
  outline: none;
  font-family: 'Century Gothic';
  width: 100%;
  min-height: 32px;
  transition-duration: 0.3s;
}

input:focus {
  border: 1.3px solid rgb(182, 182, 182);
  border-radius: 0px;
  backdrop-filter: brightness(100%);
}

.card {
  height: 200px;
  width: 340px;
  position: relative;
  transition: all 0.3s ease;
  transform-style: preserve-3d;
}

.flip .card {
  transform: rotateY(180deg);
}

.front,
.back {
  height: 100%;
  width: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
  border-radius: 24px;
  position: absolute;
}

.front {
  z-index: 2;
  backface-visibility: hidden;
}

.back {
  z-index: 1;
  transform: rotateY(180deg);
}

.container:hover .card {
  transform: rotateY(180deg);
}

.container {
  perspective: 100000px;
}
</style>
