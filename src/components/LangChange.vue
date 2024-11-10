<template>
  <v-menu>
    <template v-slot:activator="{ props }">
      <v-btn v-bind="props">言語を選択</v-btn>
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
  { title: '日本語', value: 'ja' },
  { title: '英語', value: 'en' },
  { title: 'ポルトガル語', value: 'pt' },
  { title: '中国語', value: 'zh' },
  { title: 'ロシア語', value: 'ru' },
  { title: 'ドイツ語', value: 'de' },
  { title: 'スペイン語', value: 'es' },
  { title: 'フランス語', value: 'fr' },
  { title: 'イタリア語', value: 'it' },
  { title: '翻訳を終了', value: 'off' },
];
</script>
