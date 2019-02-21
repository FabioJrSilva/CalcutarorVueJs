<template>
  <div class="calculator">
    <Display :value="displayValue"/>
    <Button label="AC" triple @onBtnClick="clearMemory"/>
    <Button label="/" operation @onBtnClick="setOperation"/>
    <Button label="7" @onBtnClick="addDigit"/>
    <Button label="8" @onBtnClick="addDigit"/>
    <Button label="9" @onBtnClick="addDigit"/>
    <Button label="*" operation @onBtnClick="setOperation"/>
    <Button label="4" @onBtnClick="addDigit"/>
    <Button label="5" @onBtnClick="addDigit"/>
    <Button label="6" @onBtnClick="addDigit"/>
    <Button label="-" operation @onBtnClick="setOperation"/>
    <Button label="1" @onBtnClick="addDigit"/>
    <Button label="2" @onBtnClick="addDigit"/>
    <Button label="3" @onBtnClick="addDigit"/>
    <Button label="+" operation @onBtnClick="setOperation"/>
    <Button label="0" double @onBtnClick="addDigit"/>
    <Button label="." @onBtnClick="addDigit"/>
    <Button label="=" operation @onBtnClick="setOperation"/>
  </div>
</template>

<script>
import Display from '../components/Display.vue'
import Button from '../components/Button.vue'

export default {
  data: function() {
    return {
      displayValue: '0',
      clearDisplay: false,
      operation: null,
      values: [0, 0],
      current: 0
    }
  },
  components: { Button, Display },
  methods: {
    clearMemory() {
      Object.assign(this.$data, this.$options.data());
    },
    setOperation(operation) {
      if (this.current === 0) {
        this.operation = operation;
        this.current = 1;
        this.clearDisplay = true;

        return 
      }

      const equals = operation === '=';
      const currentOperation = this.operation;

      try {
        this.values[0] = eval(
          `${this.values[0]} ${currentOperation} ${this.values[1]}`
        )
      } catch (error) {
        this.$emit('onError', error)
      }

      this.values[1] = 0;
      this.displayValue = this.values[0]
      this.operation = equals ? null : operation;
      this.current = equals ? 0 : 1;
      this.clearDisplay = !equals;

    },
    addDigit(n) {
      if (n === '.' && this.displayValue.includes('.')) return ;

      const clearDisplay = this.displayValue === '0'
        || this.clearDisplay;
      const currentValue = clearDisplay ? "" : this.displayValue;
      const displayValue = currentValue + n;
      this.displayValue = displayValue;
      this.clearDisplay = false;
      // Alternativa 1
      this.values[this.current] = displayValue

      // Alternativa 2
      /* if (n !== '.') {
        const index = this.current;
        const newValue = parseFloat(displayValue);
        this.values[index] = newValue;
      } */
     }
  }
}
</script>

<style>
.calculator {
  height: 320px;
  width: 235px;
  border-radius: 5px;
  overflow: hidden;

  display: grid;
  grid-template-columns: repeat(4, 25%);
  grid-template-rows: 1fr 48px 48px 48px 48px 48px;
}
</style>
