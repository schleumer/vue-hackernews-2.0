<template src="./Item.pug" lang="pug" />

<script>
export default {
  name: 'news-item',
  props: ['item'],
  // https://github.com/vuejs/vue/blob/next/packages/vue-server-renderer/README.md#component-caching
  serverCacheKey: props => {
    return `${props.item.id}::${props.item.__lastUpdated}`
  }
}
</script>
