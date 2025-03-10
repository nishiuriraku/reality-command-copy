<template>
  <v-container class="fill-height">
    <v-responsive class="align-centerfill-height mx-auto" max-width="900">
      <h1>REALITY コマンドコピー君</h1>

      <div v-for="[, value] in Object.entries(types)">
        <h2 class="my-4">{{ value }}</h2>

        <div
          v-for="v in commandArray.filter((v) => v.type === value)"
          :key="v.command"
        >
          <v-text-field
            :label="v.describe"
            :model-value="v.command"
            variant="outlined"
            append-icon="mdi-content-copy"
            @click:append="copy(v.command)"
            readonly
          >
            <template v-slot:prepend-inner>
              <v-icon
                v-if="v.permissions === 'speaker'"
                icon="mdi-account-voice"
                v-tooltip:bottom="'枠主のみ使用可能'"
              ></v-icon>
              <v-icon
                v-else
                icon="mdi-comment-account"
                v-tooltip:bottom="'視聴者も使用可能'"
              ></v-icon>
            </template>

            <template v-slot:append-inner>
              <div v-if="v.command.includes('youtube')">
                <YouTubeURL v-model="youtubeId" />
              </div>
              <div v-if="v.command.includes('title ')">
                <TitleChange v-model="title" />
              </div>
              <div v-if="v.command.includes('translate')">
                <SelectItem
                  v-model="lang"
                  :items="langItems"
                  title="言語を選択"
                />
              </div>
              <div v-if="v.command.includes('look')">
                <SelectItem
                  v-model="look"
                  :items="numItems"
                  title="番号を選択"
                />
              </div>
              <div v-if="v.command.includes('slot')">
                <SelectItem
                  v-model="slot"
                  :items="numItems"
                  title="番号を選択"
                />
              </div>
              <div v-if="v.command.includes('janken')">
                <SelectItem
                  v-model="janken"
                  :items="jankenItems"
                  title="番号を選択"
                />
              </div>
              <div v-if="v.command.includes('dice')">
                <DiceSelect v-model="dice" />
              </div>
            </template>
          </v-text-field>
        </div>
      </div>

      <v-snackbar v-model="snackbar" :timeout="timeout">
        コピーしました
      </v-snackbar>
    </v-responsive>
  </v-container>
</template>

<script setup lang="ts">
//
import { ref, computed } from 'vue';

const youtubeId = ref('');

const title = ref('');

const lang = ref('');
const langItems = [
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

const look = ref('');
const slot = ref('');
const numItems = Array.from(Array(10)).map((_, i) => {
  const n = (i + 1).toString();
  return { title: n, value: n };
});

const janken = ref('');
const jankenItems = [
  { title: 'グー (Rock)', value: 'g' },
  { title: 'チョキ (Scissor)', value: 'c' },
  { title: 'パー (Paper)', value: 'p' },
];

const dice = ref('');

enum types {
  usually = '配信系',
  transrate = '翻訳系',
  avater = 'アバター系',
  game = 'ゲーム系',
  other = 'そのほか',
}

interface Command {
  type: types;
  command: string;
  permissions: 'speaker' | 'viewer';
  describe: string;
}

// type Trump = 'b' | 'r' | '[1-5]' | 'bd[1-5]' | ' rd[1-5]';

const objectEmpty = (v: Object) => !!Object.entries(v).length;

const commandArray = computed<Command[]>(() => [
  {
    type: types.usually,
    command: '/uptime',
    permissions: 'speaker',
    describe: '配信時間確認',
  },

  {
    type: types.usually,
    command: `/youtube ${youtubeId.value || '{id}'}`,
    permissions: 'speaker',
    describe: 'YouTubeを流す',
  },
  {
    type: types.usually,
    command: '/title',
    permissions: 'speaker',
    describe: 'タイトルを確認する',
  },
  {
    type: types.usually,
    command: `/title ${title.value || '{name}'}`,
    permissions: 'speaker',
    describe: 'タイトルを変更する',
  },
  {
    type: types.transrate,
    command: `/translate ${objectEmpty(lang.value) ? lang.value : '{lang}'}`,
    permissions: 'speaker',
    describe: '指定の言語に翻訳する※実行時間に制限あり',
  },
  {
    type: types.avater,
    command: `/look ${objectEmpty(look.value) ? look.value : '{1-10}'}`,
    permissions: 'speaker',
    describe: '衣装チェンジ（その場のみ）',
  },
  {
    type: types.avater,
    command: `/slot ${objectEmpty(slot.value) ? slot.value : '{1-10}'}`,
    permissions: 'speaker',
    describe: '衣装チェンジ（着替え）',
  },
  {
    type: types.avater,
    command: '/avatar restore',
    permissions: 'speaker',
    describe: '衣装を戻す',
  },
  {
    type: types.game,
    command: '/itsudoko',
    permissions: 'speaker',
    describe: '「いつ、どこで、誰が、何をした」の文書を表示',
  },
  {
    type: types.game,
    command: '/hayakuchi',
    permissions: 'speaker',
    describe: '早口言葉を表示',
  },
  {
    type: types.game,
    command: '/omikuji',
    permissions: 'speaker',
    describe: 'おみくじ引く',
  },
  {
    type: types.game,
    command: `/dice${dice.value}`,
    permissions: 'viewer',
    describe: 'ダイスを振る',
  },
  {
    type: types.game,
    command: `/janken${objectEmpty(janken.value) ? ` ${janken.value}` : ''}`,
    permissions: 'viewer',
    describe: 'じゃんけんをする',
  },
  {
    type: types.game,
    command: '/cointoss',
    permissions: 'viewer',
    describe: 'コイントスをする',
  },
  {
    type: types.game,
    command: '/trump',
    permissions: 'viewer',
    describe: 'トランプをランダムで1枚表示',
  },
  {
    type: types.game,
    command: '/tarot',
    permissions: 'speaker',
    describe: 'タロットカードを出す',
  },

  {
    type: types.other,
    command: '/uptime game',
    permissions: 'speaker',
    describe: 'ゲーム滞在時間',
  },
  {
    type: types.other,
    command: '/uptime world',
    permissions: 'speaker',
    describe: 'ワールド滞在時間',
  },
  {
    type: types.other,
    command: '/world camera fix',
    permissions: 'speaker',
    describe: 'ワールドやルームでのカメラを固定する',
  },
  {
    type: types.other,
    command: '/world camera reset',
    permissions: 'speaker',
    describe: 'カメラの固定を解除する',
  },
  {
    type: types.other,
    command: '/world respawn',
    permissions: 'speaker',
    describe: 'ワールドの座標を初期位置に戻す',
  },
  {
    type: types.other,
    command: '/likes',
    permissions: 'speaker',
    describe: '配信者の累計いいね数を表示',
  },
  {
    type: types.other,
    command: '/ayapandance',
    permissions: 'speaker',
    describe: 'あやぱんダンスの動画を表示',
  },
  {
    type: types.other,
    command: '/metaverse',
    permissions: 'speaker',
    describe: 'REALITY公式メタバース動画を表示',
  },
  {
    type: types.other,
    command: '/neko',
    permissions: 'speaker',
    describe: '語尾が猫になる（2/22限定）',
  },
  {
    type: types.other,
    command: '/neko off',
    permissions: 'speaker',
    describe: '/neko解除',
  },
  {
    type: types.other,
    command: '/uso',
    permissions: 'speaker',
    describe: '語尾が嘘になる（4/1限定）',
  },
  {
    type: types.other,
    command: '/uso off',
    permissions: 'speaker',
    describe: '/uso解除',
  },
  {
    type: types.other,
    command: '/nominate',
    permissions: 'speaker',
    describe: '配信者かコラボユーザーの名前をランダムで表示',
  },
]);

const copy = (text: string) => {
  if (!text) return;
  navigator.clipboard.writeText(text);
  snackbar.value = true;
};

const snackbar = ref(false);
const timeout = 1000;
</script>
