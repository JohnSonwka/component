<template>
  <div class="calculator">
    <h3>JoSon</h3>
    <div class="display">
      <h5>{{signed}}{{current}}</h5>
      <h3>{{displayed || '0'}} </h3>
    </div>
    <div @click='logarithm' class="btn">ln</div>
    <div @click="percent" class="btn">%</div>
    <div @click='clear' class="btn">AC</div>
    <div class="btn operators" @click='del' >Del</div>
    <div @click="exponent" class="btn">e<sup>x</sup></div>
    <div @click="square" class="btn">nCr</div>
    <div @click='factorial' class="btn">n!</div>
    <div @click='divide' class="btn">nPr</div>
    <div @click="percent" class="btn">10<sup>x</sup></div>
    <div @click="square" class="btn">x<sup>2</sup></div>
    <div @click='power' class="btn">x<sup>y</sup></div>
    <div class="btn" >&#8730;</div>
    <div @click='inverse' class="btn">1/x</div>
    <div @click='baseLog' class="btn">log<sub>x</sub>y</div>
    <div @click='sign' class="btn">+/-</div>
    <div @click='divide' class="btn operators">/</div>
    <div @click="value(7)" class="btn">7</div>
    <div @click="value(8)" class="btn">8</div>
    <div @click="value(9)" class="btn">9</div>
    <div @click="times" class="btn operators">X</div>
    <div @click="value(4)" class="btn">4</div>
    <div @click="value(5)" class="btn">5</div>
    <div @click="value(6)" class="btn">6</div>
    <div @click="minus" class="btn operators">-</div>
    <div @click="value(1)" class="btn">1</div>
    <div @click="value(2)" class="btn">2</div>
    <div @click="value(3)" class="btn">3</div>
    <div @click="add" class="btn operators">+</div>
    <div @click="value(0)" class="btn zero">0</div>
    <div @click="dot" class="btn">.</div>
    <div @click="equal" class="btn operators">=</div>
  </div>
</template>

<script lang="ts">
export default {
  data () {
    return {
      previous: '0',
      current: '',
      operatorClicked: false,
      operator: null,
      displayed: '0',
      signed: ''
    }
  },
  methods: {
    logarithm () {
      this.previous = 'log'
      this.displayed = `${Math.log(this.current)}`
    },
    percent () {
      this.current = `${parseFloat(this.current) / 100}`
      this.displayed = this.current
    },
    clear () {
      this.current = ''
      this.previous = ''
      this.displayed = '0'
      this.signed = ''
    },
    del () {
      this.current = this.current.substr(0, this.current.length - 1)
    },
    // the exponent is in base e
    exponent () {
      this.displayed = `${Math.exp(this.current)}`
    },
    inverse () {
      this.current = 1 / this.current
    },
    square () {
      this.current *= this.current
    },
    sign () {
      if (this.current === '0') {
        return this.current
      }
      this.current = this.current.charAt(0) === '-'
        ? this.current.slice(1) : `-${this.current}`
    },
    value (n) {
      if (this.operatorClicked) {
        this.current = ''
        this.operatorClicked = false
      }
      this.current = `${this.current}${n}`
    },
    dot () {
      if (this.current.indexOf('.') === -1) {
        this.value('.')
      }
    },
    setPrevious () {
      this.previous = this.current
      this.operatorClicked = true
      this.displayed = this.current
    },
    divide () {
      this.operator = (a, b) => a / b
      this.setPrevious()
      this.signed = '/'
    },
    times () {
      this.operator = (a, b) => a * b
      this.setPrevious()
      this.signed = 'X'
    },
    minus () {
      this.operator = (a, b) => a - b
      this.setPrevious()
      this.signed = '-'
    },
    add () {
      this.operator = (a, b) => a + b
      this.setPrevious()
      this.signed = '+'
    },
    baseLog () {
      this.operator = (a, b) => Math.log(a) / Math.log(b)
      this.setPrevious()
    },
    equal () {
      if (this.current === '') {
        return
      }
      this.current = `${this.operator(
        parseFloat(this.previous),
        parseFloat(this.current)
      )}`
      this.displayed = this.current
      this.previous = null
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
.calculator{
  width: 91%;
  margin: 1%;
  padding: 5%;
  background: #999;
  border-radius: 5%;
  font-size: 24px;
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  grid-auto-rows: (50px, auto);
}
@media screen and (min-width: 800px) {
  .calculator{
  width: 35%;
  margin: auto;
  padding: 2%;
  background: #999;
  border-radius: 5%;
  font-size: 30px;
  text-align: center;
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  grid-auto-rows: (50px, auto);
  }
}

.display{
  padding: 2px;
  margin-bottom: 4px;
  border-radius: 15px 15px 10px 10px;
  grid-column: 1/5;
  background-color: rgba(248, 248, 248, 0.315);
}
h5{
  text-align: right;
  margin: 3px;
}
h3{
  margin: 0;
}
.zero{
  grid-column: 1/3;
}
.btn{
  padding: 2px;
  border-radius: 10px;
  margin: 1.5px;
  color: #eee;
  background-color: rgb(20, 7, 42);
  border: 1px solid rgb(215, 206, 231);
}
.operators{
  background-color: rgb(255, 255, 255);
  color:  rgb(20, 7, 42);
}
</style>
