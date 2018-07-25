<template>
  <select
    :class="{ inline: inline }"
    v-model="selected"
  >
    <option
      :key="period.value"
      :value="period.value"
      v-for="period in periods"
      >
      {{ period.text }}
    </option>
  </select>
</template>

<script>
export default {
  name: 'SelectPeriod',
  props: {
    inline: {
      type: Boolean,
      required: false,
      default: false
    },
    defaultSelected: {
      type: Number,
      required: true
    }
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
  watch: {
    selected(newValue, oldValue) {
      this.$emit('input', newValue);
    }
  }
}
</script>

<style scoped>
  select {
    display: block;
    font-size: 1.25rem;
    margin-top: .25rem;
  }

  select.inline {
    display: inline;
    margin-left: .25rem;
  }
</style>
