<template>
  <div class="calculator">
    <div class="calculator-display" v-text="displayValue"></div>
    <div class="calculator-keypad">
      <div class="input-keys">
        <div class="function-keys">
          <button class="calculator-key key-clear" v-on:click="clear">
            AC
          </button>
          <button class="calculator-key key-sign" v-on:click="toggleSign">
            ±
          </button>
          <button class="calculator-key key-percent">%</button>
        </div>
        <div class="digit-keys">
          <button class="calculator-key key-0" @click="calculate(0);">0</button>
          <button class="calculator-key key-dot" @click="calculate('.');">
            .
          </button>
          <button class="calculator-key key-1" @click="calculate(1);">1</button>
          <button class="calculator-key key-2" @click="calculate(2);">2</button>
          <button class="calculator-key key-3" @click="calculate(3);">3</button>
          <button class="calculator-key key-4" @click="calculate(4);">4</button>
          <button class="calculator-key key-5" @click="calculate(5);">5</button>
          <button class="calculator-key key-6" @click="calculate(6);">6</button>
          <button class="calculator-key key-7" @click="calculate(7);">7</button>
          <button class="calculator-key key-8" @click="calculate(8);">8</button>
          <button class="calculator-key key-9" @click="calculate(9);">9</button>
        </div>
      </div>
      <div class="operator-keys">
        <button
          class="calculator-key key-divide"
          @click="performOperation('/');"
        >
          ÷
        </button>
        <button
          class="calculator-key key-multiply"
          @click="performOperation('*');"
        >
          *
        </button>
        <button
          class="calculator-key key-minus"
          @click="performOperation('-');"
        >
          -
        </button>
        <button class="calculator-key key-plus" @click="performOperation('+');">
          +
        </button>
        <button
          class="calculator-key key-equals"
          @click="performOperation('=');"
        >
          =
        </button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Calculator",
  data() {
    return {
      displayValue: 0,
      isOperand: false,
      operator: null,
      preValue: null
    };
  },
  methods: {
    clear() {
      this.displayValue = 0;
      this.preValue = null;
      this.isOperand = false;
      this.operator = null;
    },
    toggleSign() {
      this.displayValue = this.displayValue * -1;
    },
    calculate(input) {
      console.log(input);

      if (this.isOperand) {
        this.displayValue = String(input);
        this.isOperand = false;
      } else {
        this.displayValue =
          this.displayValue === 0
            ? String(input)
            : this.displayValue + String(input);
      }
    },
    performOperation(operator) {
      const operations = {
        "/": (preValue, nextValue) => preValue / nextValue,
        "*": (preValue, nextValue) => preValue * nextValue,
        "-": (preValue, nextValue) => preValue - nextValue,
        "+": (preValue, nextValue) => parseInt(preValue) + parseInt(nextValue),
        "=": (preValue, nextValue) => preValue
      };
      this.isOperand = true; // to enter another values
      // to save operators
      if (this.preValue === null) {
        this.preValue = this.displayValue; // store display value to prev value for operation
      } else {
        const computedValue = operations[operator](
          this.preValue,
          this.displayValue
        );
        this.preValue = computedValue;
        this.displayValue = String(computedValue);
      }
      this.operator = operator;
    }
  }
};
</script>

<style src="../css/calculator.css"></style>
