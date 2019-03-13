<template>
  <div class="posts-list">
    <!-- <TransitionFadeSlide
      tag="div"
      direction="x"
      group
    > -->
    <PostsListItem
      v-for="post in listPosts"
      :key="post.path"
      :post="post"
    />
    <!-- </TransitionFadeSlide> -->

    <div class="posts-paginator">
      <ul v-if="total > 1">
        <li
          v-for="i in total"
          :key="i"
          :class="{
            'active': page === i,
          }"
          @click="page = i"
        >
          {{ i }}
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
// import TransitionFadeSlide from './TransitionFadeSlide.vue'
import PostsListItem from './PostsListItem.vue'

export default {
  name: 'PostsList',

  components: {
    // TransitionFadeSlide,
    PostsListItem,
  },

  props: {
    posts: {
      type: Array,
      required: false,
      default: null,
    },
  },

  data () {
    return {
      page: 1,
    }
  },

  computed: {
    pagination () {
      return this.$themeConfig.pagination || {}
    },

    perPage () {
      return this.pagination.perPage || 10
    },

    total () {
      return Math.ceil(this.allPosts.length / this.perPage)
    },

    allPosts () {
      return this.posts || this.$posts
    },

    listPosts () {
      const begin = (this.page - 1) * this.perPage
      const end = begin + this.perPage
      return this.allPosts.slice(begin, end)
    },
  },

  watch: {
    allPosts () {
      this.page = 1
    },
  },
}
</script>

<style lang="stylus" scoped>
@require '~@theme/styles/variables'

.posts-paginator
  ul
    list-style none
    li
      display inline-block
      padding 0.5rem
      &.active
        color $accentColor
        font-weight bold
      &:not(.active)
        cursor pointer
</style>
