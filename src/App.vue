<template>
  <div id="app">
    <p>CardType: {{getCreditCardType(cardNumber)}}</p>
    <input v-model="cardNumber" />
    <p>CardIsValid: {{isValidCard(cardNumber)}}</p>
    <!-- <button @click="isValidCard(cardNumber)">CHECK</button> -->
  </div>
</template>

<script>
export default {
  data: () => {
    return {
      cardNumber: "371449635398431"
    };
  },
  methods: {
    getCreditCardType(i) {
      let e = "unknown";
      return (
        /^5[1-5]/.test(i)
          ? (e = "MasterCard")
          : /^4/.test(i)
          ? (e = "Visa")
          : /^3[47]/.test(i)
          ? (e = "American Express")
          : /^(?:2131|1800|35)/.test(i)
          ? (e = "JCB")
          : /^3(?:0[0-5]|[68])/.test(i)
          ? (e = "Diners Club")
          : /^6(?:011|5)/.test(i) && (e = "Discover"),
        e
      );
    },
    getFullType(i) {
      let e = "unknown";
      return (
        /^5[1-5][0-9]{14}$/.test(i)
          ? (e = "MasterCard")
          : /^4[0-9]{12}(?:[0-9]{3})?$/.test(i)
          ? (e = "Visa")
          : /^3[47][0-9]{13}$/.test(i)
          ? (e = "American Express")
          : /^(?:2131|1800|35\d{3})\d{11}$/.test(i)
          ? (e = "JCB")
          : /^3(?:0[0-5]|[68][0-9])[0-9]{11}$/.test(i)
          ? (e = "Diners Club")
          : /^6(?:011|5[0-9]{2})[0-9]{12}$/.test(i) && (e = "Discover"),
        e
      );
    },
    calculate(e) {
      let a = 0;
      for (let i = 0; i < e.length; i++) a += parseInt(e.substring(i, i + 1));
      var n = new Array(0, 1, 2, 3, 4, -4, -3, -2, -1, 0);
      for (let i = e.length - 1; i >= 0; i -= 2) {
        let t = parseInt(e.substring(i, i + 1)),
          r = n[t];
        a += r;
      }
      let s = a % 10;
      return (s = 10 - s), 10 == s && (s = 0), s;
    },
    validate(i) {
      let e = parseInt(i.substring(i.length - 1, i.length)),
        a = i.substring(0, i.length - 1);
      return this.calculate(a) == parseInt(e) ? !0 : !1;
    },
    isValidCard(val) {
      // American Express 	371449635398431
      // Diners Club 	30569309025904
      // Discover 	6011111111111117
      // JCB 	3530111333300000
      // MasterCard 	5555555555554444
      // Visa 	4111111111111111
      let valid = this.validate(val);
      return !valid || val.length < 10 ? false : true
    }
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>