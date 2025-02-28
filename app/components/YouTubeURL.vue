<template>
  <v-dialog max-width="500">
    <template v-slot:activator="{ props: activatorProps }">
      <v-btn
        v-bind="activatorProps"
        text="URL貼り付け"
        @click="youtubeUrl = ''"
      ></v-btn>
    </template>

    <template v-slot:default="{ isActive }">
      <v-card title="URLを入力">
        <v-card-text>
          <p class="text-caption">
            ※例:<br />
            https://www.youtube.com/watch?v=XXXXXXXXXX<br />
            https://www.youtube.com/live/XXXXXXXXXX?si=ZZZZZZZZZZZZZZZ<br />
            https://youtu.be/XXXXXXXXXX?si=ZZZZZZZZZZZZZZZ
          </p>

          <v-text-field
            v-model="youtubeUrl"
            variant="outlined"
            class="mt-2"
          ></v-text-field>
        </v-card-text>

        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn text="OK" @click="isActive.value = false"></v-btn>
        </v-card-actions>
      </v-card>
    </template>
  </v-dialog>
</template>

<script setup lang="ts">
import { ref, watch } from 'vue';

const model = defineModel();

const youtubeUrl = ref('');

watch(youtubeUrl, (value) => {
  if (!value.startsWith('https://')) return (model.value = '');

  const url = new URL(value);
  const id = new URLSearchParams(url.search).get('v');

  if (id) {
    return (model.value = id);
  }
  if (url.pathname.includes('live')) {
    return (model.value = url.pathname.replace('/live/', ''));
  }
  if (url.host === 'youtu.be') {
    return (model.value = url.pathname.replace('/', ''));
  }
});
</script>
