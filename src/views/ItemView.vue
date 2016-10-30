<template src="./ItemView.pug" lang="pug" />

<script>
import Spinner from '../components/Spinner.vue'
import Comment from '../components/Comment.vue'

function fetchItem (store) {
  return store.dispatch('FETCH_ITEMS', {
    ids: [store.state.route.params.id]
  })
}

export default {
  name: 'item-view',
  components: { Spinner, Comment },
  computed: {
    item () {
      return this.$store.state.items[this.$route.params.id]
    }
  },
  preFetch: fetchItem,
  beforeMount () {
    fetchItem(this.$store)
  }
}

</script>