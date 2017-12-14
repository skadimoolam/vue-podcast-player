<template>
  <div>
    <h2 v-if="!feed.title" class="text-center">Select A Channel</h2>
    <div class="col-md-8" v-else>
      <h2>{{ feed.title || '' }}</h2>
      <h4>by. {{ feed.managingEditor || '' }}</h4>
      <p>{{ feed.description || '' }}</p>
      <a target="_blank" :href="feed.link">Link to Site</a>
      <h4 class="margin-top-30">Previous Podcasts</h4>

      <div class="list-group entry-list">
        <li class="list-group-item" v-for="entry in feed.entries" @click="play(entry.enclosure.url)">
          <span v-if="selectedTrack == entry.enclosure.url" class="pull-left padding-right-10"><i class="glyphicon glyphicon-play"></i></span>
          {{ getShortTitle(entry.title) }}
          <span class="pull-right">{{ entry.itunes.duration }}</span>
        </li>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'feedview',

  props: {
    feed: {
      type: Object,
      default: {},
      required: true
    },

    selectedTrack: {
      type: String,
      default: "",
      required: true
    }
  },

  methods: {
    play (url) {
      this.$emit("on-track-select", url)
    },

    getShortTitle(title) {
      if (!title) return "(No Title)"
      if (title.length > 90) return `${title.substr(0, 90)} ....`
      else return title
    }
  }
}
</script>

<style>
  .margin-top-30 {
    margin-top: 30px;
  }

  .padding-right-10 {
    padding-right: 10px;
  }

  .entry-list {
    border: 1px solid #d4d0d0;
    max-height: 250px;
    overflow-x: auto;
  }
</style>