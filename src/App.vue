<template>
  <div id="app">
    <label>
      Borrow
      <input
        type="number"
        v-model.number="borrowed"
      >
    </label>
    <label>
      Interest rate
      <input
        type="number"
        v-model.number="interestRate"
      >
    </label>
    <label>
      Length of loan
      <input
        type="number"
        v-model.number="loanLengthInYears"
      >
    </label>
    <label>
      Fees
      <input
        type="number"
        v-model.number="fees"
      >
    </label>

    <p class="repayments">Your repayments will be<br> {{ repayments }}</p>
  </div>
</template>

<script>

export default {
  name: 'App',
  components: {

  },
  data() {
    return {
      borrowed: 500000,
      interestRate: 5.00,
      repaymentFrequency: 12,
      loanLengthInYears: 30,
      fees: 10,
      feeFrequency: 12
    }
  },
  computed: {
    repayments() {
      let monthlyRate = parseFloat(this.interestRate) / 100 / 12;
      let loanLengthInMonths = this.loanLengthInYears * 12;
      let repayment = this.borrowed * (monthlyRate * Math.pow(1 + monthlyRate, loanLengthInMonths)) / (Math.pow(1 + monthlyRate, loanLengthInMonths) - 1);
      return `$${Math.round(repayment + this.fees)} per month`;
    }
  }
}
</script>

<style scoped>
  #app {
    font-family: "Helvetica Neue", sans-serif;
    margin: 2rem auto;
    width: 30rem;
  }

  label, input {
    display: block;
    font-size: 1.25rem;
  }

  label {
    margin-bottom: 1rem;
  }

  input {
    margin-top: .25rem;
    padding: .25rem;
    text-align: right;
    width: 7rem;
  }

  .repayments {
    font-size: 2rem;
  }
</style>
