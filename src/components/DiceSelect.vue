<template>
  <v-dialog max-width="500">
    <template v-slot:activator="{ props: activatorProps }">
      <v-btn v-bind="activatorProps" text="種類を選択"></v-btn>
    </template>

    <template v-slot:default="{ isActive }">
      <v-card title="ダイスの種類を選択">
        <v-card-text>
          <v-radio-group v-model="radios">
            <v-radio
              v-for="v in radioValue"
              :key="v.label"
              :label="v.label"
              :value="v.value"
            ></v-radio>
          </v-radio-group>

          <div v-if="radios === 2">
            <v-slider
              v-model="numbers"
              step="10"
              thumb-label
              show-ticks="always"
            >
              <template v-slot:append>
                <v-text-field
                  v-model="numbers"
                  piecesd
                  density="compact"
                  style="width: 100px"
                  type="number"
                  variant="outlined"
                  hide-details
                ></v-text-field>
              </template>
            </v-slider>
          </div>

          <div v-else-if="radios === 3">
            <p class="text-caption">面</p>
            <v-slider v-model="faces" step="1" min="1" thumb-label>
              <template v-slot:append>
                <v-text-field
                  v-model="faces"
                  density="compact"
                  style="width: 100px"
                  type="number"
                  variant="outlined"
                  hide-details
                ></v-text-field>
              </template>
            </v-slider>

            <p class="text-caption">個数</p>
            <v-slider
              v-model="pieces"
              step="1"
              max="10"
              min="1"
              thumb-label
              show-ticks="always"
            >
              <template v-slot:append>
                <v-text-field
                  v-model="pieces"
                  density="compact"
                  style="width: 100px"
                  type="number"
                  variant="outlined"
                  hide-details
                ></v-text-field>
              </template>
            </v-slider>
          </div>
        </v-card-text>

        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn
            text="OK"
            @click="
              isActive.value = false;
              ok();
            "
          ></v-btn>
        </v-card-actions>
      </v-card>
    </template>
  </v-dialog>
</template>

<script setup lang="ts">
import { ref } from 'vue';
const model = defineModel();

const radioValue = [
  { label: '普通に振る', value: 1 },
  { label: '数字を指定する', value: 2 },
  { label: '個数と面を指定する', value: 3 },
];
const radios = ref(1);
const numbers = ref(1);

const faces = ref(100);
const pieces = ref(1);

const ok = () => {
  switch (radios.value) {
    case 2:
      model.value = ` ${numbers.value}`;
      break;
    case 3:
      model.value = ` ${pieces.value}d${faces.value}`;
      break;
    default:
      model.value = ``;
      break;
  }
};
</script>
