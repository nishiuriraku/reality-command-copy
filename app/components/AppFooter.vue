<script setup lang="ts">
const icons = [{ icon: 'mdi-home', link: 'https://nishiuriraku.netlify.app' }];
const linkToOtherWindow = (url: string) => window.open(url, '_blank');
</script>

<template>
  <v-footer class="d-flex flex-column">
    <div>
      <v-btn
        v-for="i in icons"
        :key="i.icon"
        :icon="i.icon"
        class="mx-4"
        size="small"
        variant="plain"
        @click="linkToOtherWindow(i.link)"
      ></v-btn>
    </div>
    <div>
      © {{ new Date().getFullYear() }} nishiuriraku. All rights reserved.
    </div>
  </v-footer>
</template>
