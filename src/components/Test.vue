<template>
  <div class="m-3">
    Test
    <div>No Ref : {{ number + 1 }}</div>
    <div>Ref : {{ refNumber }}</div>
    <div>Reactive : {{ reactiveNumber }}</div>
  </div>
</template>
<script>
import { ref, reactive,toRefs } from "vue";
export default {
  setup() {
    let number = 1;
    const refNumber = ref(1);
    const reactiveNumber = reactive({ count: 1 });
    const {count} = toRefs(reactiveNumber)
    setInterval(() => {
      number += 1;
      refNumber.value += 1;
      reactiveNumber.count += 1;
    }, 1000);
    return { number, refNumber, reactiveNumber };
  },
};
</script>