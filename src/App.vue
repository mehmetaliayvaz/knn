<template>
  <div class="p-5">
    
    <div class="mb-5">
      <div class="d-flex">
        <div v-for="(columnItem, columnIndex) in data" :key="columnIndex" class="d-flex flex-column col-1 me-4">
          <input type="text" v-model="label[columnIndex]" class="mb-5">
          <input v-for="(rowItem, rowIndex) in data[columnIndex]" :key="rowIndex" v-model="data[columnIndex][rowIndex]" type="text" class="mb-2">
          <div>
            <button class="btn-danger me-2" @click="data[columnIndex].pop()">-</button>
            <button class="btn-success" @click="data[columnIndex].push('')">+</button>
          </div>
        </div>
        <button class="btn-danger me-2" @click="data.pop(), resultData.pop(), label.pop()">-</button>
        <button class="btn-success" @click="data.push([]), resultData.push([]), label.push('')">+</button>
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
      <button class="btn-secondary me-2" @click="row.pop(), resultData.forEach(item => item.pop())">-</button>
      <button class="btn-primary" @click="row.push(''), resultData.forEach(item => item.push(''))">+</button>
    </div>

    <button class="my-5" @click="calc()">Hesapla</button><br>

    {{ data }}<br>
    {{ resultData }}
  </div>
</template>

<script setup>
import { ref } from "vue";
 
const label = ref([]);

const data = ref([[]]);

const resultData = ref([[]]);

const row = ref([]);

const mainEntropy = ref();

const calc = () => {
  const mainData = data.value[data.value.length-1];
  const temp = mainData[0];
  const count = 0;
  mainData.slice(1).forEach(item => {
    if(temp == item){
      count++;
    }
    temp = item;
  })

}
 
</script>

<style>

</style>
