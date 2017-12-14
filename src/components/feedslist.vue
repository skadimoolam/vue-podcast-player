<template>
  <div class="col-md-4">
    <div class="list-group" v-if="feeds != {}">
      <li class="list-group-item" v-for="feed in feeds" @click="select(feed.title)" :class="{active: feed.title == selectedFeedTitle}">
        {{ getShortTitle(feed.title) }}
      </li>
    </div>
    <div v-else>No Podcast urls found</div>
  </div>
</template>

<script>
export default {
  name: 'feedslist',

  props: {
    feeds: {
      type: Object,
      default: {},
      required: true
    },

    selectedFeedTitle: {
      type: String,
      default: "",
      required: true
    }
  },

  methods: {
    select (name) {
      this.$emit("on-feed-select", name)
    },

    getShortTitle(title) {
      if (!title) return "(No Title)"
      if (title.length > 35) return `${title.substr(0, 35)} ....`
      else return title
    }
  }
}
</script>

<style>
  li.list-group-item {
    cursor: pointer !important;
  }
</style>