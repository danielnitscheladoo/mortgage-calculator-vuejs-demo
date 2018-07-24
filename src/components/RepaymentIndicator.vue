<template>
  <p class="repayments">Your repayments will be<br> <span>{{ repayments }}</span></p>
</template>

<script>

export default {
  name: 'RepaymentIndicator',
  props: {
    interestRate: {
      type: Number,
      required: true,
    },
    loanLengthInYears: {
      type: Number,
      required: true
    },
    borrowed: {
      type: Number,
      required: true
    },
    fees: {
      type: Number,
      required: true
    },
    feeFrequency: {
      type: Number,
      required: true
    },
    repaymentFrequency: {
      type: Number,
      required: true
    },
  },
  data () {
    return {
      periods: [
        { text: 'Yearly', value: 1 },
        { text: 'Quarterly', value: 4 },
        { text: 'Monthly', value: 12 },
        { text: 'Fortnightly', value: 26 },
        { text: 'Weekly', value: 52 },
      ],
      selected: this.defaultSelected
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
  .repayments {
    font-size: 2rem;
  }

  span {
    color: salmon;
  }
</style>
