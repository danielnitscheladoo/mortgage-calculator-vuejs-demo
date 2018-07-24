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

    <repayment-indicator
      :interest-rate="interestRate"
      :loan-length-in-years="loanLengthInYears"
      :borrowed="borrowed"
      :fees="fees"
      :feeFrequency="feeFrequency"
      :repaymentFrequency="repaymentFrequency"
    />
  </div>
</template>

<script>
import SelectPeriod from './components/SelectPeriod';
import RepaymentIndicator from './components/RepaymentIndicator';

export default {
  name: 'App',
  components: {
    SelectPeriod,
    RepaymentIndicator
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
