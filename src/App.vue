
<template>
  <section class="text-gray-600 body-font">
    <div class="py-10 lg:py-24 mx-auto">
      <div class="flex flex-col text-center w-full mb-5">
        <h1 class="sm:text-3xl text-2xl font-medium title-font mb-4 text-gray-900">Conversor de Moedas</h1>
        </div>
      <div class="flex lg:w-2/3 w-full sm:flex-row flex-col mx-auto px-8 sm:space-x-4 sm:space-y-0 space-y-4 sm:px-0 items-end">
        <div class="flex-col w-full">
          <div class="items-center flex justify-center mb-5">
            <p @click="changeDollar" :class="{'bg-green-600 hover:bg-green-700':inputDollar,  'bg-red-600 hover:bg-red-700':!inputDollar}"
               class="px-5 py-2 rounded text-white cursor-pointer ">
              <span v-if="!inputDollar">Converter Dólar para Real </span>
              <span v-else >Converter Real para Dólar</span>
            </p>
          </div>
          <div class="items-center flex justify-center ">
            <div v-if="inputDollar" class="">
                <label for="full-name" class="leading-7 text-sm text-red-600">$ - Dólar</label>
                <Currency :class="{'border-red-500':inputDollar}" v-model="dolar" :options="{ currency: 'USD'}" placeholder="$ "/>
            </div>
            <div v-if="!inputDollar" class="">
              <label for="full-name" class="leading-7 text-sm text-green-600">R$ - Real </label>
              <Currency :class="{'border-green-500':!inputDollar}" v-model="real" :options="{ currency: 'BRL'}" placeholder="R$"/>
            </div>
          </div>
        </div>
      </div>

      <hr class="my-5" :class="{'border-green-500':!inputDollar,  'border-red-500':inputDollar}">

      <section class="text-gray-600 body-font">
        <div class="container px-5 py-8 mx-auto">
          <div class="flex flex-wrap -m-4 text-center">
            <div class="p-4 w-full lg:w-1/2">
              <p class="leading-relaxed">O Dólar hoje está</p>
              <h2 class="title-font font-medium sm:text-4xl text-3xl text-gray-900">{{dolarTodayValue}}</h2>
            </div>

            <div v-if="inputDollar" class="p-4  w-full lg:w-1/2">
              <p class="leading-relaxed">{{dolarToReaisisValue}}</p>
              <h2 class="title-font font-medium sm:text-4xl text-3xl text-green-600">{{realInDolar}}</h2>
            </div>

            <div v-else class="p-4  w-full lg:w-1/2">
              <p class="leading-relaxed">{{realToDolaresisValue}}</p>
              <h2 class="title-font font-medium sm:text-4xl text-3xl text-red-600">{{dolarInReal}}</h2>
            </div>

          </div>
        </div>
      </section>
    </div>
  </section>


</template>

<script setup>
import {computed, onMounted, ref, watch} from 'vue'
import Currency from '@/components/Currency.vue'
import format from './service/format'

const dolar = ref(0);
const real = ref(0);
const dolarToday = ref(0);
const dolarToReal = ref(0);
const realtoDolar = ref(0);
const inputDollar = ref(false);

const dolarToReaisisValue = computed( () => {
  return `Valor em reais de ${format(dolar.value, 'en-US', 'USD')} é `
})

const realToDolaresisValue = computed( () => {
  return `Valor em dólar de ${format(real.value, 'pt-BR', 'BRL')} é `
})

const  dolarTodayValue = computed( () => {
  return format(dolarToday.value, 'en-US', 'USD');
})

function changeDollar() {
  inputDollar.value = !inputDollar.value;

}

const realInDolar = computed(() => {
  return format(dolarToReal.value, 'pt-BR', 'BRL')
})

const dolarInReal = computed(() => {
  return format(realtoDolar.value, 'en-US', 'USD')
})

onMounted(async () => {
  try {
    const dolar = await getDolar();
    dolarToday.value = dolar.high
  }catch (err){
    console.log(err)
  }
})
async function getDolar(typeCurrency = 'USD-BRL'){
  const res = await fetch('https://economia.awesomeapi.com.br/json/last/'+typeCurrency)
      .then(res => res.json())
      return res.USDBRL;
}

watch(dolar, (value) =>{
  dolarToReal.value = value * +dolarToday.value
})

watch(real, (value) =>{
  realtoDolar.value = value / +dolarToday.value
})

</script>
