<template>
  <h2 class="ps-5 pt-3 text-center">KNN Algoritması</h2>
  <hr class="mb-0">
  <div class="d-flex">
    <div class="w-50 p-5 me-5 border-end">
      
      <div style="margin-bottom: 7rem;">
        <div class="d-flex">
          <div v-for="(columnItem, columnIndex) in data" :key="columnIndex" class="d-flex flex-column me-4">
            <input type="text" v-model="label[columnIndex]" class="mb-3 form-control" :placeholder="columnIndex == data.length-1 ? 'Durum' : 'Özellik'">
            <hr>
            <input v-for="(rowItem, rowIndex) in data[columnIndex]" :key="rowIndex" v-model="data[columnIndex][rowIndex]" type="text" class="form-control mb-2">
            <div>
              <button class="btn btn-danger me-2" @click="data[columnIndex].pop()">-</button>
              <button class="btn btn-success" @click="data[columnIndex].push(null)">+</button>
            </div>
          </div>
          <button class="btn btn-danger me-2" @click="data.pop(), resultData.pop(), label.pop(), calcData.pop()">-</button>
          <button class="btn btn-success" @click="data.push([]), resultData.push([]), label.push(null), calcData.push(null)">+</button>
        </div>
      </div>

    </div>

    <div class="w-50 p-5">
      <div class="mb-5">
        <div class="d-flex mb-3">
          <div v-for="(columnItem, columnIndex) in data" :key="columnIndex" class=" me-4">
            <label class="mb-2">{{ label[columnIndex] }}</label>
            <select v-for="(rowItem, rowIndex) in row" :key="rowIndex" v-model="resultData[columnIndex][rowIndex]" class="form-select mb-2">
              <option v-for="(item, index) in data[columnIndex]" :key="index">{{ item }}</option>
            </select>
          </div>
        </div>
        <button class="btn btn-secondary me-2" @click="row.pop(), resultData.forEach(item => item.pop())">-</button>
        <button class="btn btn-primary" @click="row.push(null), resultData.forEach(item => item.push(null))">+</button>
      </div>

      <div class="d-flex">
        <div class="mb-5 me-5 d-flex flex-column">
          <h4 class="mb-3">Ölçüm Yapılacak Veri</h4>
          <div class="d-flex">
            <div v-for="(columnItem, columnIndex) in data.slice(0, -1)" :key="columnIndex" class="d-flex flex-column me-4">
              <label>{{ label[columnIndex] }}</label>
              <input v-model="calcData[columnIndex]" type="text" class="form-control">
            </div>
          </div>
        </div>
        <div>
          <h4 class="mb-3">K Değeri</h4>
          <label></label>
          <input type="number" v-model="kData" class="form-control">
        </div>
      </div>

      <button  @click="calc()" class="btn btn-info mb-3">Hesapla</button>
      <div class="d-flex">
        <div class="me-5">
          <p v-for="(item, index) in tempData" :key="index" :class="index < kData ? 'text-success fw-bold' : ''">
            {{ item.data + ' - ' + item.state }}
          </p>
        </div>
        <h3 v-if="result">Sonuç: {{result}} </h3>
      </div>
    </div>

  </div>

  <button @click="clearData()" class="btn btn-warning btn-delete m-5">Tüm Verileri Sil</button>
</template>

<script setup>
import { ref, watch } from "vue";
 
const label = ref(JSON.parse(localStorage.getItem('label')) ?? []);

const data = ref(JSON.parse(localStorage.getItem('data')) ?? [[]]);

const resultData = ref(JSON.parse(localStorage.getItem('resulData')) ?? [[]]);

const row = ref(JSON.parse(localStorage.getItem('row')) ?? []);

const calcData = ref(JSON.parse(localStorage.getItem('calcData')) ?? []);

const tempData = ref([]);

const result = ref("");

const kData = ref();

const calc = () => {
  tempData.value = [];

  var temp = {
    data : 0.0,
    state: '',
  };

  for(var i = 0; i < row.value.length; i++){

    for(var k = 0; k < resultData.value.length - 1; k++){

      temp.data += Math.pow((parseFloat(resultData.value[k][i]) - parseFloat(calcData.value[k])), 2);
    }
  
    temp.data = (Math.sqrt(temp.data)).toFixed(2);
    temp.state = resultData.value[resultData.value.length - 1][i];

    tempData.value.push(temp);
    temp = {data: 0.0, state: ''};

  }

  tempData.value.sort((a,b) => a.data - b.data)

  const newData = tempData.value.slice(0, kData.value).map(item => item.state);
  const counts = {};
  newData.forEach((x) => { counts[x] = (counts[x] || 0) + 1 });
  result.value = Object.keys(counts)[Object.values(counts).indexOf(Math.max(...Object.values(counts)))]
  
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

.form-control, .form-select{
  width: 120px !important;
}

.btn{
  max-height: 40px !important;
}

.btn-delete{
  position: fixed;
  bottom: 0px;
  left: 0px;
}

</style>
