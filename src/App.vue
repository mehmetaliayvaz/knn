<template>
  <div class="p-5">

    <div class="mb-5">
      <input v-for="(columnItem, columnIndex) in data.slice(0, -1)" :key="columnIndex" v-model="calcData[columnIndex]" type="text" class="col-1 me-4">
    </div>
    
    <div class="mb-5">
      <div class="d-flex">
        <div v-for="(columnItem, columnIndex) in data" :key="columnIndex" class="d-flex flex-column col-1 me-4">
          <input type="text" v-model="label[columnIndex]" class="mb-5">
          <input v-for="(rowItem, rowIndex) in data[columnIndex]" :key="rowIndex" v-model="data[columnIndex][rowIndex]" type="text" class="mb-2">
          <div>
            <button class="btn btn-danger me-2" @click="data[columnIndex].pop()">-</button>
            <button class="btn btn-success" @click="data[columnIndex].push(null)">+</button>
          </div>
        </div>
        <button class="btn btn-danger me-2" @click="data.pop(), resultData.pop(), label.pop(), calcData.pop()">-</button>
        <button class="btn btn-success" @click="data.push([]), resultData.push([]), label.push(null), calcData.push(null)">+</button>
      </div>
    </div>

    <div class="mb-5">
      <div class="d-flex mb-3">
        <div v-for="(columnItem, columnIndex) in data" :key="columnIndex" class="col-1 me-4">
          <span>{{ label[columnIndex] }}</span>
          <select v-for="(rowItem, rowIndex) in row" :key="rowIndex" v-model="resultData[columnIndex][rowIndex]" class="w-100">
            <option v-for="(item, index) in data[columnIndex]" :key="index">{{ item }}</option>
          </select>
        </div>
      </div>
      <button class="btn btn-secondary me-2" @click="row.pop(), resultData.forEach(item => item.pop())">-</button>
      <button class="btn btn-primary" @click="row.push(null), resultData.forEach(item => item.push(null))">+</button>
    </div>

    <button class="my-5" @click="calc()">Hesapla</button>{{ tempData }}
    
    <br>


     {{ resultData }}<br>
     {{ calcData }}<br>


    <button @click="clearData()" class="my-5">Tüm verileri sil.</button>
  </div>
</template>

<script setup>
import { ref, watch } from "vue";
 
const label = ref(JSON.parse(localStorage.getItem('label')) ?? []);

const data = ref(JSON.parse(localStorage.getItem('data')) ?? [[]]);

const resultData = ref(JSON.parse(localStorage.getItem('resulData')) ?? [[]]);

const row = ref(JSON.parse(localStorage.getItem('row')) ?? []);

const calcData = ref(JSON.parse(localStorage.getItem('calcData')) ?? []);

const tempData = ref([]);

const calc = () => {
  var temp = 0.0;
  var tempArray = [];

  for(var i = 0; i < row.value.length; i++){

    for(var k = 0; k < resultData.value.length - 1; k++){

      temp += Math.pow((parseFloat(resultData.value[k][i]) - parseFloat(calcData.value[k])), 2);
    }
  
    tempArray.push(Math.sqrt(temp));
    temp = 0;

  }

  tempData.value.push(tempArray);
  tempArray = [];
  
}


const clearData = () => {
  localStorage.clear();
  label.value = [];
  data.value = [[]];
  resultData.value = [[]];
  row.value = [];
  calcData.value = [];
}


watch( label.value, () => {
  localStorage.setItem('label', JSON.stringify(label.value));
});

watch( data.value, () => {
  localStorage.setItem('data', JSON.stringify(data.value));
});

watch( resultData.value, () => {
  localStorage.setItem('resulData', JSON.stringify(resultData.value));
});

watch( row.value, () => {
  localStorage.setItem('row', JSON.stringify(row.value));
});

watch( calcData.value, () => {
  localStorage.setItem('calcData', JSON.stringify(calcData.value));
});

</script>

<style>

</style>
