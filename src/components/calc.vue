<template>
  <div class="calculator">
    <div class="display">
        <input type="text" v-model="currentValue" disabled />
      </div>
      <div class="buttons">
        <button @click="appendNumber(7)">7</button>
        <button @click="appendNumber(8)">8</button>
        <button @click="appendNumber(9)">9</button>
        <button @click="operate('/')">/</button>
  
        <button @click="appendNumber(4)">4</button>
        <button @click="appendNumber(5)">5</button>
        <button @click="appendNumber(6)">6</button>
        <button @click="operate('*')">*</button>
  
        <button @click="appendNumber(1)">1</button>
        <button @click="appendNumber(2)">2</button>
        <button @click="appendNumber(3)">3</button>
        <button @click="operate('-')">-</button>
  
        <button @click="appendNumber(0)">0</button>
        <button @click="clear">C</button>
        <button @click="calculate">=</button>
        <button @click="operate('+')">+</button>
      </div>
  </div>
</template>


<script setup>
import { ref } from 'vue';

const currentValue = ref(''); // برای نگهداری مقدار وارد شده

let operator = ref('');
let previousValue = ref('');

// اضافه کردن عدد به مقدار جاری
const appendNumber = (num) => {
  currentValue.value += num;
};

// تنظیم عملیات (جمع، تفریق، ضرب، تقسیم)
const operate = (op) => {
  if (currentValue.value === '') return; // اگر هیچ عددی وارد نشده باشد، عملیات انجام نشود
  operator.value = op;
  previousValue.value = currentValue.value;
  currentValue.value = '';
};

// محاسبه نتیجه
const calculate = () => {
  if (previousValue.value === '') return;
  const current = parseFloat(currentValue.value);
  const prev = parseFloat(previousValue.value);

  let result;
  switch (operator.value) {
    case '+':
      result = prev + current;
      break;
    case '-':
      result = prev - current;
      break;
    case '*':
      result = prev * current;
      break;
    case '/':
      if (current === 0) {
        result = 'خطای تقسیم بر صفر';
      } else {
        result = prev / current;
      }
      break;
    default:
      return;
  }
  currentValue.value = result.toString();
  previousValue.value = '';
  operator.value = '';
};

// پاک کردن مقدار جاری
const clear = () => {
  currentValue.value = '';
  previousValue.value = '';
  operator.value = '';
};
</script>

<style scoped>
.calculator {
  max-width: 260px;
  margin: 0 auto;
  border: 2px solid #ccc;
  padding: 20px;
  border-radius: 10px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}

</style>
