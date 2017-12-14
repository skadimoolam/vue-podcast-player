<template>
  <div class="container">
    <h1>Plain Podcast Player <span class="pull-right"><player :track="selectedTrack"/></span></h1>
    <hr>

    <div class="row">
      <feedslist :feeds="feeds" :selected-feed-title="selectedFeed.title" @on-feed-select="feedSelected"/>
      <feedview :feed="selectedFeed" :selected-track="selectedTrack" @on-track-select="trackSelected"/>
    </div>
  </div>
</template>

<script>
import { parseURL } from 'rss-parser'

import feedslist from '@/components/feedslist.vue'
import feedview from '@/components/feedview.vue'
import player from '@/components/player.vue'

export default {
  components: { feedslist, feedview, player },

  data() {
    return {
      feeds: {},
      selectedFeed: {},
      selectedTrack: ""
    }
  },

  methods: {
    feedSelected (name) {
      if (!this.feeds[name]) this.selectedFeed = {}
      this.selectedFeed = this.feeds[name]
    },

    trackSelected (url) {
      if (!url) this.selectedTrack = ""
      this.selectedTrack = url
    }
  },

  mounted() {
    fetch("/static/feeds.json")
      .then(res => res.json())
      .then(res => {
        if (res.length) {
          res.forEach(feed => {
            parseURL(feed, (err, data) => {
              if (err) console.error(err)
              this.$set(this.feeds, data.feed.title, data.feed)
            })

          })
        } else {
          alert("No Podcast urls found")
        }
      })

  }
}
</script>