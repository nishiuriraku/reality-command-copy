<template>
  <v-dialog max-width="500">
    <template v-slot:activator="{ props: activatorProps }">
      <v-btn
        v-bind="activatorProps"
        text="タイトル変更"
        @click="model = ''"
      ></v-btn>
    </template>

    <template v-slot:default="{ isActive }">
      <v-card title="タイトルを入力">
        <v-card-text>
          <v-text-field
            v-model="model"
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
const model = defineModel();
</script>
