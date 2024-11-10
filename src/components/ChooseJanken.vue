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
        <v-list-item-title>{{ item.title }}</v-list-item-title>
      </v-list-item>
    </v-list>
  </v-menu>
</template>

<script setup lang="ts">
const model = defineModel();

const items = [
  { title: 'グー (Rock)', value: 'g' },
  { title: 'チョキ (Scissor)', value: 'c' },
  { title: 'パー (Paper)', value: 'p' },
];
</script>
