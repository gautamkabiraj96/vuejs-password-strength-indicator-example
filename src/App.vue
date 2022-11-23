<template>
  <div id="app">
    <h2>Password strength indicator example in Vue.js</h2>
    <div>
      <input
        name="passwordInput"
        type="password"
        placeholder="Enter password"
        v-model="inputValue"
        @keyup="checkInputStrength"
      />
    </div>
    <div class="np-password-hint">
      <small>
        Password should be at least 8 characters long and contain a number and a
        symbol.
      </small>
    </div>
    <h4>Password strength</h4>
    <div class="np-password-strength-indicator-container">
      <div
        class="np-password-strength-indicator"
        :style="{
          backgroundColor: getIndicatorBackgroundColor(),
          width: getIndicatorWidth() + '%',
        }"
      ></div>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      inputValue: null,
      inputStrength: 0,
    };
  },
  methods: {
    checkInputStrength() {
      this.inputStrength = 0;

      const inputStrengthLengthCheck =
        this.inputValue && this.inputValue.length >= 8;
      const inputContainsSpecialCharacters = this.inputContainsSpecialCharacters();
      const inputContainsNumbers = this.inputContainsNumbers();

      if (inputStrengthLengthCheck) {
        this.inputStrength++;
      }
      if (inputContainsSpecialCharacters) {
        this.inputStrength++;
      }
      if (inputContainsNumbers) {
        this.inputStrength++;
      }
    },
    inputContainsSpecialCharacters() {
      const specialCharacters = /[`!@#$%^&*()_+\-=\\|,.<>?~]/;
      return specialCharacters.test(this.inputValue);
    },
    inputContainsNumbers() {
      const numbers = /\d/;
      return numbers.test(this.inputValue);
    },
    getIndicatorBackgroundColor() {
      let color = "gray";
      switch (this.inputStrength) {
        case 0:
        case 1:
          color = "red";
          break;
        case 2:
          color = "orange";
          break;
        case 3:
          color = "green";
          break;
        default:
          color = "gray";
      }

      return color;
    },
    getIndicatorWidth() {
      return parseInt((this.inputStrength / 3) * 100).toString();
    },
  },
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #000000;
  margin: 60px;
}
input {
  font-size: 16px;
  padding: 4px;
  border: 1px solid rgb(34, 34, 34);
  outline: none;
}
.np-password-strength-indicator-container {
  width: 300px;
  height: 20px;
  background: #eee;
  border-radius: 6px;
}
.np-password-strength-indicator {
  width: 300px;
  height: 20px;
  background: #eee;
  border-radius: 6px;
  width: 33%;
  transition: all 0.3s;
}
.np-password-hint {
  margin-top: 10px;
  max-width: 300px;
}
</style>
