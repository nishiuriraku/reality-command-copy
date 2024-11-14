<template>
  <v-menu>
    <template v-slot:activator="{ props }">
      <v-btn v-bind="props">{{ title }}</v-btn>
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

type Props = {
  items: { title: string; value: string }[];
  title: string;
};
const { items, title } = defineProps<Props>();
</script>
