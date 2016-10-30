<template src="./UserView.pug" lang="pug" />

<script>
import Spinner from '../components/Spinner.vue'

function fetchUser (store) {
  return store.dispatch('FETCH_USER', {
    id: store.state.route.params.id
  })
}

export default {
  name: 'user-view',
  components: { Spinner },
  computed: {
    user () {
      return this.$store.state.users[this.$route.params.id]
    }
  },
  preFetch: fetchUser,
  beforeMount () {
    fetchUser(this.$store)
  }
}
</script>
