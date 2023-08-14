<script setup>
import { ref } from 'vue';
const props = defineProps(['row']);

// `row' as the initial value; disconnected from future prop updates
const values = ref(props.row);

console.log(values.value);
</script>


<template>
  <tr>
    <td v-for="(value, index) in values">
      <template v-if="index == 5">
        <input v-model="values[index]"/>
      </template>
      <template v-else>
        {{ value }}
      </template>
    </td>
  </tr>
    <!-- <td><input v-model="b"/> </td>
    <td><input v-model="c"/> </td>
    <td><input v-model="d"/> </td>
    <td>{{ q }}</td> -->
  <!-- <div class="item">
    <i>
      <slot name="icon"></slot>
    </i>
    <div class="details">
      <h3>
        <slot name="heading"></slot>
      </h3>
      <slot></slot>
    </div>
  </div> -->
</template>

<style scoped>
</style>
