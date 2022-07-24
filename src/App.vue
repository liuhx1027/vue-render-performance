<script setup lang="ts">
import PlotChat from './components/PlotChat.vue';
import { computed, onMounted, ref, watch } from "vue"
import LineChat from './components/LineChat.vue';
import AreaChat from './components/AreaChat.vue';
import debounce from "lodash/debounce";

const inputValue = ref("");
const debouncedInputValue = ref("");
const optionValue = ref<"synchronous" | "debounced" | "asynchronous">("synchronous")

const debouncedInputHandler = debounce((newValue: string) => {
  debouncedInputValue.value = newValue
}, 500);

watch([inputValue], () => {
  debouncedInputHandler(inputValue.value);
})

</script>

<template>
  <div className="p-3">
    <div className="text-center">
      <span>
        <input type="radio" id="optionSync" value="synchronous" name="renderOption" v-model="optionValue" />
        <label for="optionSync"> Synchronous </label>
      </span>
      <span>
        <input type="radio" id="optionDebounced" value="debounced" name="renderOption" v-model="optionValue" />
        <label for="optionDebounced"> Debounced </label>
      </span>
      <span>
        <input disabled type="radio" id="optionAsynchronous" value="asynchronous" name="renderOption"
          v-model="optionValue" />
        <label for="optionAsynchronous"> Asynchronous </label>
      </span>
    </div>
    <div>
      <input className="border rounded w-full" type="text" v-model="inputValue"
        placeholder="longer you type => the more node it will generate" />
    </div>
    <div>
      current value:
      <span>{{ optionValue === 'debounced' ? debouncedInputValue : inputValue }}</span> =>
      <span>{{ optionValue }}</span>
    </div>
    <div className="container flex flex-wrap">
      <PlotChat className="basis-1/2 overflow-hidden"
        :input="optionValue === 'debounced' ? debouncedInputValue : inputValue" />
      <LineChat className="basis-1/2 overflow-hidden"
        :input="optionValue === 'debounced' ? debouncedInputValue : inputValue" />
      <AreaChat className="basis-1/1 overflow-hidden"
        :input="optionValue === 'debounced' ? debouncedInputValue : inputValue" />
    </div>
  </div>
</template>
<style>
div.container {
  background-color: rgb(34, 34, 34);
}
</style>
