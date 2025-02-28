<template>
  <NuxtLayout>
    <NuxtPage />
  </NuxtLayout>
</template>

<script setup lang="ts">
const title = 'REALITY コマンドコピー君';
const description =
  '顔出しナシのライブ配信アプリ「REALITY」で使えるコマンドのまとめ。1タップで使いたいコマンドをコピーできるような便利ツールです。';
const url = 'https://reality-command-copy.netlify.app/';
const ogp = url + 'ogp.png';

useHead({
  title,
  meta: [{ name: 'description', content: description }],
  link: [{ rel: 'manifest', href: '/manifest.json' }],
});

useSeoMeta({
  title,
  ogTitle: title,
  description,
  ogDescription: description,
  ogUrl: url,
  ogImage: ogp,
  ogImageWidth: '1200',
  ogImageHeight: '630',
  ogType: 'website',
  ogSiteName: title,
  ogLocale: 'ja_JP',
  twitterCard: 'summary',
  twitterCreator: '@nishiuriraku',
  twitterTitle: title,
  twitterDescription: description,
  twitterImage: ogp,
});
</script>
