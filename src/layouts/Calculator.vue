<template>
  <div class="calc">
    <div class="display">
      <DisplayScreen :displayValue="displayValue" />
      <ResultScreen :resultValue="resultValue"/>
    </div>
    <div class="keypad">
      <div class="keypad-row">
        <Button label="AC" value="AC" doubleSpace @handleClick="cleanAll" />
        <Button label="C" value="C" @handleClick="clean"/>
        <Button label="÷" value="/" operation @handleClick="setOperation"/>
      </div>
      <div class="keypad-row">
        <Button label="7" value="7" @handleClick="digit" />
        <Button label="8" value="8" @handleClick="digit" />
        <Button label="9" value="9" @handleClick="digit" />
        <Button label="x" value="*" operation @handleClick="setOperation" />
      </div>
      <div class="keypad-row">
        <Button label="4" value="4" @handleClick="digit" />
        <Button label="5" value="5" @handleClick="digit" />
        <Button label="6" value="6" @handleClick="digit" />
        <Button label="-" value="-" operation @handleClick="setOperation" />
      </div>
      <div class="keypad-row">
        <Button label="1" value="1" @handleClick="digit" />
        <Button label="2" value="2" @handleClick="digit" />
        <Button label="3" value="3" @handleClick="digit" />
        <Button label="+" value="+" operation @handleClick="setOperation" />
      </div>
      <div class="keypad-row">
        <Button label="0" value="0" @handleClick="digit" />
        <Button label="." value="." @handleClick="digit" />
        <Button label="+/-" @handleClick="changeSign" />
        <Button label="=" value="=" operation @handleClick="equal" />
      </div>
    </div>
  </div>
</template>

<script>
import DisplayScreen from '../components/DisplayScreen'
import ResultScreen from '../components/ResultScreen'
import Button from '../components/Button'
import { evaluate } from 'mathjs'

export default {
  name: 'Calculator',
  components: {
    DisplayScreen,
    ResultScreen,
    Button
  },
  data: function () {
    return {
      displayValue: '0',
      resultValue: '',
      values: [''],
      maxDigit: false,
      operation: false,
      sign: false,
      err: false
    }
  },
  methods: {
    digit (digit) {
      if (digit === '.' && this.values[this.values.length - 1].includes('.')) return

      if (this.values[this.values.length - 1].indexOf('.') !== -1) {
        const decimal = this.values[this.values.length - 1].indexOf('.')
        if (decimal - (this.values[this.values.length - 1].length - 1) === -3) return
      }

      this.displayValue = (this.displayValue === '0' && digit === '.') ? this.displayValue + digit
        : (this.displayValue === '0' && digit !== '.') ? digit
          : (this.displayValue.slice(-1) === ' ' && digit === '.') ? this.displayValue + '0' + digit
            : this.displayValue + digit

      this.values = this.displayValue.split(/([-+*/])/g)

      if (this.values.length >= 3) this.equation()
    },
    setOperation (op) {
      this.operation = true
      this.maxDigit = false

      if (this.operation) {
        if (['+', '-', '*', '/'].includes(this.displayValue.slice(-1))) {
          const valueReplaced = this.displayValue.slice(-1).replace(this.displayValue.slice(-1), op)
          this.displayValue = this.displayValue.slice(0, -1) + valueReplaced
          return
        }

        this.displayValue = `${this.displayValue}${op}`
        const values = this.displayValue.split(/([-+*/])/g)
        this.values = values.filter(v => v)
      }
    },
    equation () {
      if (this.operation) {
        if (['+', '-', '*', '/'].includes(this.displayValue.slice(-1))) return

        const value = evaluate(this.displayValue)
        const result = Number.isInteger(value) ? value.toString() : value.toFixed(3).toString()
        this.resultValue = parseFloat(result).toString()
      }
    },
    equal () {
      if (['+', '-', '*', '/'].includes(this.displayValue.slice(-1))) return

      const value = evaluate(this.displayValue)
      const result = Number.isInteger(value) ? value.toString() : value.toFixed(3).toString()
      this.displayValue = parseFloat(result).toString()
      this.values = this.displayValue.split(/([-+*/])/g)
      this.resultValue = ''
    },
    changeSign () {
      if (['+', '-', '*', '/'].includes(this.displayValue.slice(-1))) return
      if (this.displayValue === '0') return

      const value = this.values[this.values.length - 1]
      const indexValue = this.values.length - 1

      const sign = Math.sign(value)
      if (sign === 1) {
        this.values = this.values.map((e, index) => {
          return index === indexValue ? `(-${value})` : e
        })
      } else {
        this.values = this.values.map((e, index) => {
          return index === indexValue ? value.match(/[\d.]+/g).pop() : e
        })
      }

      this.displayValue = this.values.join('')
      this.equation()
    },
    clean () {
      if (this.displayValue.length === 0) return

      this.displayValue = this.displayValue.substr(0, this.displayValue.length - 1)
      this.values = this.displayValue.split(/([-+*/])/g)
      this.values = this.values.filter(item => item)

      if (this.values.length === 0) this.values = ['']

      if (this.displayValue.length < 1) this.displayValue = '0'
      if (this.values.length < 3) this.resultValue = ''
      if (this.values.length >= 3) this.equation()
    },
    cleanAll () {
      this.displayValue = '0'
      this.resultValue = ''
      this.values = ['']
      this.maxDigit = false
      this.operation = false
      this.sign = false
    }
  }
}
</script>

<style>
  .calc{
    display: flex;
    flex-direction: column;
    width: 300px;
    height: 460px;
    border-radius: 8px;
    background-color: #cdcdcd;
    overflow: hidden;
    box-shadow: 0 14px 28px rgba(0, 0, 0, 0.4);
  }

  .display {
    display: flex;
    flex-direction: column;
    width: 100%;
    height: 140px;
    background-color: #09203f;
    padding: 10px;
  }

  .keypad {
    display: flex;
    flex-direction: column;
    width: 100%;
  }

  .keypad-row {
    display: flex;
    flex-direction: row;
    height: 64px;
    width: 100%;
  }
</style>
