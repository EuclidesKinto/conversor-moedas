<template>
  <input class="flex-grow pl-2 pr-3 border block py-2 px-0
            rounded text-md text-gray-900 border-b-2
            focus:outline-none focus:ring-0" ref="inputRef" type="text"/>
</template>

<script setup>
import  {useCurrencyInput} from 'vue-currency-input';
const props = defineProps(['modelValue', 'options']);
// const inputRef = ref(null);
const  {inputRef } = useCurrencyInput(props.options)
</script>
