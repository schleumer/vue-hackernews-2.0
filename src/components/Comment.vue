<template src="./Comment.pug" lang="pug" />

<script>
export default {
  name: 'comment',
  props: ['id'],
  data () {
    return {
      open: true
    }
  },
  computed: {
    comment () {
      return this.$store.state.items[this.id]
    }
  },
  beforeMount () {
    this.$store.dispatch('FETCH_ITEMS', {
      ids: [this.id]
    })
  },
  methods: {
    pluralize (n) {
      return n + (n === 1 ? ' reply' : ' replies')
    }
  }
}
</script>