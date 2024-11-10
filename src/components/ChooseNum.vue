<template>
  <v-menu>
    <template v-slot:activator="{ props }">
      <v-btn v-bind="props">番号を選択</v-btn>
    </template>
    <v-list v-model:selected="model">
      <v-list-item
        v-for="(item, index) in items"
        :key="index"
        :value="item.value"
      >
        <v-list-item-title>{{ item.value }}</v-list-item-title>
      </v-list-item>
    </v-list>
  </v-menu>
</template>

<script setup lang="ts">
const model = defineModel();

const items = [
  { value: '1' },
  { value: '2' },
  { value: '3' },
  { value: '4' },
  { value: '5' },
  { value: '6' },
  { value: '7' },
];
</script>
