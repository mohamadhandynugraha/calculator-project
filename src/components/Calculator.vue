<template>
  <div>
    <div class="calculator">
      <el-row>
        <el-col class="calculator__input" :span="24">
          <span @keyup="onKeyup">{{ visibleValue }}</span>
        </el-col>
      </el-row>
      <el-row>
        <el-col class="calculator__output padding__left--16" :span="1"
          >=</el-col
        >
        <el-col class="calculator__output" :span="20">{{ result }}</el-col>
        <el-col class="horizontal__line" :span="24">
          <hr />
        </el-col>
      </el-row>
      <el-row padding__x>
        <el-col :span="18">
          <el-button
            class="calculator__key calculator__key--ac calculator__color__keypad"
            type="danger"
            @click.native="clear"
            >CLEAR
          </el-button>
        </el-col>
        <el-col class="padding__left--8" :span="6">
          <Operator
            class="calculator__key calculator__key--operator calculator__color__keypad"
            value="x"
            action="multiply"
            v-on:operate="operate"
          ></Operator>
        </el-col>
      </el-row>
      <el-row padding__x :gutter="10">
        <el-col :span="6"
          ><Operand class="calculator__key" value="1"></Operand
        ></el-col>
        <el-col :span="6"
          ><Operand class="calculator__key" value="2"></Operand
        ></el-col>
        <el-col :span="6"
          ><Operand class="calculator__key" value="3"></Operand
        ></el-col>
        <el-col :span="6"
          ><Operator
            class="calculator__key calculator__key--operator calculator__color__keypad"
            value="+"
            action="add"
            v-on:operate="operate"
          ></Operator
        ></el-col>
      </el-row>
      <el-row padding__x :gutter="10">
        <el-col :span="6"
          ><Operand class="calculator__key" value="4"></Operand
        ></el-col>
        <el-col :span="6"
          ><Operand class="calculator__key" value="5"></Operand
        ></el-col>
        <el-col :span="6"
          ><Operand class="calculator__key" value="6"></Operand
        ></el-col>
        <el-col :span="6"
          ><Operator
            class="calculator__key calculator__key--operator calculator__color__keypad"
            value="-"
            action="subtract"
            v-on:operate="operate"
          ></Operator
        ></el-col>
      </el-row>
      <el-row padding__x :gutter="10">
        <el-col :span="6"
          ><Operand class="calculator__key" value="7"></Operand
        ></el-col>
        <el-col :span="6"
          ><Operand class="calculator__key" value="8"></Operand
        ></el-col>
        <el-col :span="6"
          ><Operand class="calculator__key" value="9"></Operand
        ></el-col>
        <el-col :span="6"
          ><Operator
            class="calculator__key calculator__key--operator calculator__color__keypad"
            value="/"
            action="divide"
            v-on:operate="operate"
          ></Operator
        ></el-col>
      </el-row>
      <el-row padding__x :gutter="10">
        <el-col :span="6"
          ><button class="calculator__key calculator__key--operator" @click="toggleSign">
            +/-
          </button></el-col
        >
        <el-col :span="6"
          ><Operand class="calculator__key" value="0"></Operand
        ></el-col>
        <el-col :span="6"
          ><button class="calculator__key calculator__key--operator">
            .
          </button></el-col
        >
        <el-col :span="6"
          ><button
            @click="calculate"
            class="calculator__key calculator__key--operator"
          >
            =
          </button></el-col
        >
      </el-row>
    </div>
  </div>
</template>

<script>
import Operator from "./Operator";
import Operand from "./Operand";
import { mapState, mapActions } from "vuex";

export default {
  name: "Calculator",
  components: {
    Operator,
    Operand,
  },
  computed: mapState(["visibleValue", "result"]),
  methods: {
    ...mapActions(["clear", "calculate"]),
    operate(action) {
      this.$store.dispatch(action);
    },
    toggleSign () {
      this.$store.dispatch('toggleSign')
    },
    onKeyup(event) {
      let value = event.target.value;
      this.$store.dispatch("setOperandValue", value);
    },
  },
};
</script>

<style scoped>
.padding__left--8 {
  padding-left: 8px;
}
.padding__left--16 {
  padding-left: 16px;
}
.horizontal__line {
  padding-left: 16px;
  padding-right: 16px;
}
.padding__x {
  padding-left: 24px;
  padding-right: 8px;
}
.calculator {
  border-radius: 10px;
  box-shadow: 0px 3px 6px 0px rgba(0, 0, 0, 0.15),
    0px 2px 4px 0px rgba(0, 0, 0, 0.12);
  margin-inline-start: auto;
  margin-inline-end: auto;
  margin-block-start: 2em;
  max-inline-size: 22rem;
  overflow: hidden;
  color: hsl(202, 11%, 29%);
}
.calculator__color__keypad {
  color: #000010;
}
.calculator__input {
  background: hsl(255, 100%, 100%);
  font-size: 2.1rem;
  padding-block-start: 3rem;
  padding-block-end: 0.5rem;
  padding-inline-end: 1.25rem;
  text-align: end;
}
.calculator__output {
  background: hsl(255, 100%, 100%);
  font-size: 4.2rem;
  padding-block-start: 3rem;
  padding-block-end: 0.5rem;
  padding-inline-end: 1.25rem;
  text-align: end;
}
.calculator__keys {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  grid-gap: 1px;
  background: hsl(255, 100%, 100%);
  padding: 0.5rem;
}
.calculator__key {
  background: #DEF3F4;
  border: none;
  padding-block-start: 1rem;
  padding-block-end: 1rem;
  padding-inline-end: 1.25rem;
  padding-inline-start: 1.25rem;
  font-size: 1.5rem;
  /* Playful Styles */
  inline-size: 70px;
  block-size: 70px;
  margin: 0.25rem;
  border-radius: 40px;
}
.calculator__key:active,
.calculator__key:focus {
  box-shadow: 0 0 8px 0 rgba(0, 0, 0, 0.3) inset;
  outline: none;
}
.calculator__key--operator {
  background: #FFF6DB;
}
.calculator__key--operator:active {
  background: hsl(208, 24%, 80%);
}
.calculator__key--enter {
  grid-column: 4 / 5;
  grid-row: 2 / span 4;
  background: hsl(357, 100%, 72%);
  height: auto;
}
.calculator__key--ac {
  width: 250px;
  background: #DEF3F4;
  height: auto;
}
</style>