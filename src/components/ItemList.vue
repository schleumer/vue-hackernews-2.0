<template src="./ItemList.pug" lang="pug"/>

<script>
import Spinner from './Spinner.vue'
import Item from './Item.vue'
import { watchList } from '../store/api'

export default {
  name: 'item-list',

  components: {
    Spinner,
    Item
  },

  props: {
    type: String
  },

  data () {
    const isInitialRender = !this.$root._isMounted
    return {
      loading: false,
      transition: 'slide-left',
      // if this is the initial render, directly render with the store state
      // otherwise this is a page switch, start with blank and wait for data load.
      // we need these local state so that we can precisely control the timing
      // of the transitions.
      displayedPage: isInitialRender ? Number(this.$store.state.route.params.page) || 1 : -1,
      displayedItems: isInitialRender ? this.$store.getters.activeItems : []
    }
  },

  computed: {
    page () {
      return Number(this.$store.state.route.params.page) || 1
    },
    maxPage () {
      const { itemsPerPage, lists } = this.$store.state
      return Math.ceil(lists[this.type].length / itemsPerPage)
    },
    hasMore () {
      return this.page < this.maxPage
    }
  },

  beforeMount () {
    // if data is already fetched on server-side, just ignore.
    if(!this.$store.state.fetched || this.$root._isMounted)
        this.loadItems(this.page)

    // watch the current list for realtime updates
    this.unwatchList = watchList(this.type, ids => {
      this.$store.commit('SET_LIST', { type: this.type, ids })
      this.$store.dispatch('ENSURE_ACTIVE_ITEMS').then(() => {
        this.displayedItems = this.$store.getters.activeItems
      })
    })
  },

  mounted() {

  },

  beforeDestroy () {
    this.unwatchList()
  },

  watch: {
    page (to, from) {
      this.loadItems(to, from)
    }
  },

  methods: {
    loadItems (to = this.page, from = -1) {
      this.loading = true
      this.$store.dispatch('FETCH_LIST_DATA', {
        type: this.type
      }).then(() => {
        if (this.page < 0 || this.page > this.maxPage) {
          this.$router.replace(`/${this.type}/1`)
          return
        }
        this.transition = to > from ? 'slide-left' : 'slide-right'
        this.displayedPage = to
        this.displayedItems = this.$store.getters.activeItems
        this.loading = false
      })
    }
  }
}



</script>