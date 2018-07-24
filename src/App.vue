<template>
  <div id="app">
    <label>
      Borrow
      <input
        type="number"
        v-bind:value="borrowed"
        v-on:input="borrowed = parseFloat($event.target.value)"
      >
    </label>
    <label>
      Interest rate
      <input
        type="number"
        :value="interestRate"
        @input="interestRate = parseFloat($event.target.value)"
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
      Repayment frequency
      <select-period
        :default-selected="repaymentFrequency"
        @update="repaymentFrequency = parseFloat($event.target.value)"
      />
    </label>
    <label>
      Fees
      <div class="inline">
        <input
          class="inline"
          type="number"
          v-model.number="fees"
        >
        <select-period
          :inline="true"
          :default-selected="feeFrequency"
          v-model="feeFrequency"
        />
      </div>
    </label>

    <p class="repayments">Your repayments will be<br> {{ repayments }}</p>
  </div>
</template>

<script>
import SelectPeriod from './components/SelectPeriod';

export default {
  name: 'App',
  components: {
    SelectPeriod
  },
  data() {
    return {
      borrowed: 500000,
      interestRate: 5.00,
      repaymentFrequency: 12,
      loanLengthInYears: 30,
      fees: 10,
      feeFrequency: 1
    }
  },
  computed: {
    repayments() {
      let monthlyRate = parseFloat(this.interestRate) / 100 / 12;
      let loanLengthInMonths = this.loanLengthInYears * 12;
      let monthlyRepayment = this.borrowed * (monthlyRate * Math.pow(1 + monthlyRate, loanLengthInMonths)) / (Math.pow(1 + monthlyRate, loanLengthInMonths) - 1);
      let monthlyRepaymentWithFees = monthlyRepayment + ((this.fees * this.feeFrequency) / 12);
      // Not the actual maths, just faking this part.
      let repaymentPerPeriod = (monthlyRepaymentWithFees * 12) / this.repaymentFrequency;
      return `$${Math.round(repaymentPerPeriod)} per ${this.getPeriodName(this.repaymentFrequency)}`;
    }
  },
  methods: {
    getPeriodName(value) {
      const periods = [
        { text: 'year', value: 1 },
        { text: 'quarter', value: 4 },
        { text: 'month', value: 12 },
        { text: 'fortnight', value: 26 },
        { text: 'week', value: 52 },
      ];
      return periods.find(period => period.value === value).text;
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

  .inline input {
    display: inline-block;
  }

  .repayments {
    font-size: 2rem;
  }
</style>
