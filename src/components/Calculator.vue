<template>
  <div class="dailer">
    <div class="line">
      <div class="result-area">
        <ul v-for="c in pastCalculations" v-bind:key="c.formula">
          <li>
            <span class="left">{{c.formula}}</span> =
            <span class="right" v-on:click="addToFormula(c.result)">{{c.format}}</span>
          </li>
        </ul>
      </div>
      <input class="full-width" type="text" v-model="calculate">
    </div>

    <div class="line">
      <div class="btn" v-on:click="addToFormula(1)">1</div>
      <div class="btn" v-on:click="addToFormula(2)">2</div>
      <div class="btn" v-on:click="addToFormula(3)">3</div>
      <div class="btn" v-on:click="addToFormula('+')">+</div>
    </div>
    
    <div class="line">
      <div class="btn" v-on:click="addToFormula(4)">4</div>
      <div class="btn" v-on:click="addToFormula(5)">5</div>
      <div class="btn" v-on:click="addToFormula(6)">6</div>
      <div class="btn" v-on:click="addToFormula('-')">-</div>
    </div>
    
    <div class="line">
      <div class="btn" v-on:click="addToFormula(7)">7</div>
      <div class="btn" v-on:click="addToFormula(8)">8</div>
      <div class="btn" v-on:click="addToFormula(9)">9</div>
      <div class="btn" v-on:click="addToFormula('*')">x</div>
    </div>

    <div class="line">
      <div class="btn" v-on:click="addToFormula(0)">0</div>
      <div class="btn" v-on:click="emptyFormula">C</div>
      <div class="btn" v-on:click="calc">=</div>
      <div class="btn" v-on:click="addToFormula('/')">/</div>
    </div>
  </div>
</template>

<script>
import numeral from 'numeral'

export default {
  name: 'Calculator',
  data () {
    return {
      calculate: '',
      result: 0,
      pastCalculations: [],
      resetFormula: false,
      variables: []
    }
  },
  methods: {
    calc () {
      // eslint-disable-next-line
      this.result = eval(this.calculate)
      this.pastCalculations.push({
        formula: this.calculate,
        result: this.result,
        format: numeral(this.result).format('0,0.[00]')
      })
      this.addToFormula(' = ' + this.result)
      this.resetFormula = true
    },
    addToFormula (sym) {
      if (this.resetFormula) {
        this.emptyFormula()
        this.resetFormula = false
      }

      this.calculate = this.calculate + sym
    },
    emptyFormula () {
      this.calculate = ''
    },
    addVariable (key, value) {
      this.variables.push({ key, value })
    }
  }
}
</script>

<style>
.dailer {
  box-sizing: border-box;
}

.line {
  width: 100%;
  box-sizing: border-box;
}

.line > .btn {
  width: 25%;
  height: 50px;
  border: 1px solid yellowgreen;
  float: left;
  box-sizing: border-box;
  line-height: 50px;
  cursor: pointer;
  transition: background-color 0.2s ease-in;
}

.btn:hover {
  background-color: #00FF00;
}
.full-width { 
  width: 100%;
  margin: 0 -6px;
  box-sizing: border-box;
  height: 30px;
}

.result-area {
  height: 50px;
  overflow-y: auto;
}

.result-area > ul {
  list-style-type: none;
  margin: 0;
  padding: 0;
}

.left {
  text-align: left;
  width: 70%;
  display: inline-block;
  border: 1px solid red;
}
.right {
  text-align: right;
  border: 1px solid green;
  min-width: 200px;
  display: inline-block;
}
</style>
